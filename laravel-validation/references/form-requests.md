# Form Requests Reference

Complete reference for Laravel Form Request classes (Laravel 12.x).

## Table of Contents

- [Creating a Form Request](#creating-a-form-request)
- [Available Methods & Properties](#available-methods--properties)
- [Authorization](#authorization)
- [Preparing Input](#preparing-input)
- [Conditional & Dynamic Rules](#conditional--dynamic-rules)
- [After Validation Hooks](#after-validation-hooks)
- [Error Customization](#error-customization)
- [Patterns](#patterns)

---

## Creating a Form Request

```bash
php artisan make:request StorePostRequest
```

Generated class at `app/Http/Requests/StorePostRequest.php`:

```php
namespace App\Http\Requests;

use Illuminate\Foundation\Http\FormRequest;

class StorePostRequest extends FormRequest
{
    public function authorize(): bool
    {
        return true; // or Gate/policy check
    }

    public function rules(): array
    {
        return [
            'title' => ['required', 'string', 'max:255'],
            'body' => ['required', 'string'],
        ];
    }
}
```

Type-hint in controller to auto-validate:

```php
public function store(StorePostRequest $request): RedirectResponse
{
    $validated = $request->validated();
    Post::create($validated);
    return redirect()->route('posts.index');
}
```

---

## Available Methods & Properties

| Method / Property | Description |
|-------------------|-------------|
| `authorize(): bool` | Return `true` to allow, `false` to abort with 403 |
| `rules(): array` | Define validation rules |
| `messages(): array` | Custom error messages (e.g., `'title.required' => '...'`) |
| `attributes(): array` | Custom names for the `:attribute` placeholder |
| `prepareForValidation(): void` | Mutate input before validation runs |
| `passedValidation(): void` | Hook that runs after validation passes |
| `after(): array` | Return array of after-validation callbacks |
| `$stopOnFirstFailure = true` | Property: stop on first rule failure (any field) |

---

## Authorization

```php
public function authorize(): bool
{
    // Simple check
    return $this->user()->can('create', Post::class);

    // Using route model binding
    return $this->user()->can('update', $this->route('post'));
}
```

When `authorize()` returns `false`, a 403 response is returned automatically.

---

## Preparing Input

Sanitize or transform input before validation:

```php
protected function prepareForValidation(): void
{
    $this->merge([
        'slug' => Str::slug($this->slug),
        'is_published' => $this->boolean('is_published'),
    ]);
}
```

After validation passes:

```php
protected function passedValidation(): void
{
    $this->replace([
        'name' => ucfirst($this->validated('name')),
    ]);
}
```

---

## Conditional & Dynamic Rules

### Rules based on request context

```php
public function rules(): array
{
    return [
        'email' => [
            'required', 'email',
            // Different unique rule for create vs update
            $this->route('user')
                ? Rule::unique('users')->ignore($this->route('user'))
                : Rule::unique('users'),
        ],
    ];
}
```

### Rules based on input values

```php
public function rules(): array
{
    return [
        'type' => ['required', Rule::in(['personal', 'business'])],
        'company_name' => [Rule::requiredIf($this->type === 'business')],
        'vat_number' => [Rule::requiredIf(fn () => $this->type === 'business')],
    ];
}
```

### Shared rules between create/update

Extract a trait or base class, or use a single Form Request with conditional logic:

```php
class SavePostRequest extends FormRequest
{
    public function rules(): array
    {
        $rules = [
            'title' => ['required', 'string', 'max:255'],
            'body' => ['required', 'string'],
        ];

        if ($this->isMethod('POST')) {
            $rules['slug'] = ['required', 'unique:posts'];
        }

        return $rules;
    }
}
```

---

## After Validation Hooks

For cross-field or complex validation that doesn't fit standard rules:

```php
// Using the after() method (recommended)
public function after(): array
{
    return [
        function (Validator $validator) {
            if ($this->somethingIsInvalid()) {
                $validator->errors()->add('field', 'Something is wrong.');
            }
        },
        new ValidateShippingAddress,  // invokable class
    ];
}
```

```php
// Using withValidator (alternative)
public function withValidator(Validator $validator): void
{
    $validator->after(function (Validator $validator) {
        if ($this->invalidCombination()) {
            $validator->errors()->add('general', 'Invalid field combination.');
        }
    });
}
```

---

## Error Customization

### Custom messages

```php
public function messages(): array
{
    return [
        'title.required' => 'Every post needs a title.',
        'body.required' => 'The post body cannot be empty.',
        'tags.*.max' => 'Each tag may not exceed :max characters.',
    ];
}
```

### Custom attribute names

Replace `:attribute` placeholder in default messages:

```php
public function attributes(): array
{
    return [
        'email' => 'email address',
        'hp' => 'phone number',
        'dob' => 'date of birth',
    ];
}
```

---

## Patterns

### Store vs Update requests

```php
// StorePostRequest
public function rules(): array
{
    return [
        'title' => ['required', 'string', 'max:255'],
        'slug' => ['required', 'string', 'unique:posts,slug'],
        'body' => ['required', 'string'],
    ];
}

// UpdatePostRequest
public function rules(): array
{
    return [
        'title' => ['sometimes', 'required', 'string', 'max:255'],
        'slug' => [
            'sometimes', 'required', 'string',
            Rule::unique('posts', 'slug')->ignore($this->route('post')),
        ],
        'body' => ['sometimes', 'required', 'string'],
    ];
}
```

### Nested/array validation in Form Request

```php
public function rules(): array
{
    return [
        'items' => ['required', 'array', 'min:1'],
        'items.*.product_id' => ['required', 'exists:products,id'],
        'items.*.quantity' => ['required', 'integer', 'min:1'],
        'items.*.options' => ['nullable', 'array'],
        'items.*.options.color' => ['nullable', 'string'],
        'items.*.options.size' => ['nullable', Rule::in(['S', 'M', 'L', 'XL'])],
    ];
}
```

### Accessing route parameters

```php
public function rules(): array
{
    return [
        'email' => [
            'required', 'email',
            Rule::unique('users')->ignore($this->route('user')),
        ],
    ];
}
```

### Using `validated()` vs `safe()`

```php
// In controller after Form Request
public function store(StorePostRequest $request)
{
    // Get all validated data as array
    $data = $request->validated();

    // Get ValidatedInput object for chainable operations
    $safe = $request->safe();
    $postData = $safe->only(['title', 'body']);
    $metaData = $safe->except(['title', 'body']);
    $withExtra = $safe->merge(['author_id' => auth()->id()]);
}
```
