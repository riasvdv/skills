# Rule Class & Fluent Builders Reference

Complete reference for `Illuminate\Validation\Rule` and all fluent rule builder classes (Laravel 12.x).

## Table of Contents

- [Rule Factory Methods](#rule-factory-methods)
- [Unique](#unique)
- [Exists](#exists)
- [Dimensions](#dimensions)
- [Enum](#enum)
- [Password](#password)
- [File & ImageFile](#file--imagefile)
- [Date](#date)
- [Numeric](#numeric)
- [Email](#email)
- [In & NotIn](#in--notin)
- [Conditional & Logic](#conditional--logic)

---

## Rule Factory Methods

All methods on `Illuminate\Validation\Rule` are static and return fluent builder instances.

| Method | Signature | Returns |
|--------|-----------|---------|
| `unique` | `unique(string $table, string $column = 'NULL')` | `Rules\Unique` |
| `exists` | `exists(string $table, string $column = 'NULL')` | `Rules\Exists` |
| `in` | `in(Arrayable\|UnitEnum\|array\|string $values)` | `Rules\In` |
| `notIn` | `notIn(Arrayable\|UnitEnum\|array\|string $values)` | `Rules\NotIn` |
| `enum` | `enum(class-string $type)` | `Rules\Enum` |
| `dimensions` | `dimensions(array $constraints = [])` | `Rules\Dimensions` |
| `file` | `file()` | `Rules\File` |
| `imageFile` | `imageFile(bool $allowSvg = false)` | `Rules\ImageFile` |
| `date` | `date()` | `Rules\Date` |
| `dateTime` | `dateTime()` | `Rules\Date` (pre-formatted) |
| `email` | `email()` | `Rules\Email` |
| `numeric` | `numeric()` | `Rules\Numeric` |
| `anyOf` | `anyOf(array $rules)` | `Rules\AnyOf` |
| `array` | `array(array\|null $keys = null)` | `Rules\ArrayRule` |
| `can` | `can(string $ability, ...$arguments)` | `Rules\Can` |
| `contains` | `contains(Arrayable\|UnitEnum\|array\|string $values)` | `Rules\Contains` |
| `doesntContain` | `doesntContain(Arrayable\|UnitEnum\|array\|string $values)` | `Rules\DoesntContain` |
| `forEach` | `forEach(callable $callback)` | `NestedRules` |
| `requiredIf` | `requiredIf(Closure\|bool $callback)` | `Rules\RequiredIf` |
| `excludeIf` | `excludeIf(Closure\|bool $callback)` | `Rules\ExcludeIf` |
| `prohibitedIf` | `prohibitedIf(Closure\|bool $callback)` | `Rules\ProhibitedIf` |
| `when` | `when(callable\|bool $condition, $rules, $defaultRules = [])` | `ConditionalRules` |
| `unless` | `unless(callable\|bool $condition, $rules, $defaultRules = [])` | `ConditionalRules` |

---

## Unique

`Rule::unique(string $table, string $column = 'NULL')`

| Method | Signature | Description |
|--------|-----------|-------------|
| `ignore` | `ignore($id, string $idColumn = null)` | Exclude a record by ID or Model |
| `ignoreModel` | `ignoreModel(Model $model, string $idColumn = null)` | Exclude an Eloquent model |
| `where` | `where(Closure\|string $column, $value = null)` | Add WHERE clause |
| `whereNot` | `whereNot(string $column, $value)` | Add WHERE NOT clause |
| `whereNull` | `whereNull(string $column)` | Add WHERE NULL clause |
| `whereNotNull` | `whereNotNull(string $column)` | Add WHERE NOT NULL clause |
| `whereIn` | `whereIn(string $column, array $values)` | Add WHERE IN clause |
| `whereNotIn` | `whereNotIn(string $column, array $values)` | Add WHERE NOT IN clause |
| `withoutTrashed` | `withoutTrashed(string $deletedAtColumn = 'deleted_at')` | Ignore soft-deleted records |
| `onlyTrashed` | `onlyTrashed(string $deletedAtColumn = 'deleted_at')` | Only check soft-deleted records |
| `using` | `using(Closure $callback)` | Custom query callback |

```php
'email' => [
    Rule::unique('users', 'email')
        ->ignore($user->id)
        ->withoutTrashed()
        ->where('account_id', $accountId),
],
```

---

## Exists

`Rule::exists(string $table, string $column = 'NULL')`

Shares all query methods with Unique (except `ignore`/`ignoreModel`):

| Method | Signature | Description |
|--------|-----------|-------------|
| `where` | `where(Closure\|string $column, $value = null)` | Add WHERE clause |
| `whereNot` | `whereNot(string $column, $value)` | Add WHERE NOT clause |
| `whereNull` | `whereNull(string $column)` | Add WHERE NULL clause |
| `whereNotNull` | `whereNotNull(string $column)` | Add WHERE NOT NULL clause |
| `whereIn` | `whereIn(string $column, array $values)` | Add WHERE IN clause |
| `whereNotIn` | `whereNotIn(string $column, array $values)` | Add WHERE NOT IN clause |
| `withoutTrashed` | `withoutTrashed(string $deletedAtColumn = 'deleted_at')` | Ignore soft-deleted records |
| `onlyTrashed` | `onlyTrashed(string $deletedAtColumn = 'deleted_at')` | Only check soft-deleted records |
| `using` | `using(Closure $callback)` | Custom query callback |

```php
'department_id' => [
    Rule::exists('departments', 'id')
        ->where('active', true)
        ->withoutTrashed(),
],
```

---

## Dimensions

`Rule::dimensions(array $constraints = [])`

| Method | Signature | Description |
|--------|-----------|-------------|
| `width` | `width(int $value)` | Exact width in pixels |
| `height` | `height(int $value)` | Exact height in pixels |
| `minWidth` | `minWidth(int $value)` | Minimum width |
| `maxWidth` | `maxWidth(int $value)` | Maximum width |
| `minHeight` | `minHeight(int $value)` | Minimum height |
| `maxHeight` | `maxHeight(int $value)` | Maximum height |
| `ratio` | `ratio(float $value)` | Exact aspect ratio |
| `minRatio` | `minRatio(float $value)` | Minimum aspect ratio |
| `maxRatio` | `maxRatio(float $value)` | Maximum aspect ratio |
| `ratioBetween` | `ratioBetween(float $min, float $max)` | Aspect ratio in range |

```php
'avatar' => [
    'required',
    Rule::imageFile(),
    Rule::dimensions()
        ->minWidth(100)
        ->minHeight(100)
        ->maxWidth(2000)
        ->ratio(1),
],
```

---

## Enum

`Rule::enum(class-string $type)`

| Method | Signature | Description |
|--------|-----------|-------------|
| `only` | `only(UnitEnum[]\|UnitEnum\|Arrayable $values)` | Whitelist specific cases |
| `except` | `except(UnitEnum[]\|UnitEnum\|Arrayable $values)` | Blacklist specific cases |

```php
'status' => [Rule::enum(OrderStatus::class)->except([OrderStatus::Cancelled])],
```

---

## Password

`Password::min(int $size)` or `Password::default()`

**Static methods:**

| Method | Signature | Description |
|--------|-----------|-------------|
| `min` | `Password::min(int $size)` | Create builder with minimum length |
| `default` | `Password::default()` | Use previously configured defaults |
| `defaults` | `Password::defaults(Closure $callback)` | Set application-wide defaults |
| `required` | `Password::required()` | Shortcut for `['required', Password::default()]` |
| `sometimes` | `Password::sometimes()` | Shortcut for `['sometimes', Password::default()]` |

**Chainable methods:**

| Method | Signature | Description |
|--------|-----------|-------------|
| `max` | `max(int $size)` | Maximum length |
| `letters` | `letters()` | Require at least one letter |
| `mixedCase` | `mixedCase()` | Require upper and lowercase |
| `numbers` | `numbers()` | Require at least one number |
| `symbols` | `symbols()` | Require at least one symbol |
| `uncompromised` | `uncompromised(int $threshold = 0)` | Check Have I Been Pwned API |
| `rules` | `rules(Closure\|string\|array $rules)` | Merge additional rules |

```php
// Set defaults in AppServiceProvider::boot
Password::defaults(fn () => Password::min(8)->letters()->mixedCase()->uncompromised());

// Use in validation
'password' => ['required', 'confirmed', Password::default()],
```

---

## File & ImageFile

`Rule::file()` or `File::types(...)` or `Rule::imageFile()`

**Static methods:**

| Method | Signature | Description |
|--------|-----------|-------------|
| `File::types` | `File::types(string\|array $mimetypes)` | Create builder with allowed MIME types |
| `File::image` | `File::image()` | Create ImageFile builder |
| `File::default` | `File::default()` | Use previously configured defaults |

**Chainable methods:**

| Method | Signature | Description |
|--------|-----------|-------------|
| `extensions` | `extensions(string\|array $extensions)` | Restrict file extensions |
| `size` | `size(string\|int $size)` | Exact file size (e.g., `'1mb'`, `512`) |
| `min` | `min(string\|int $size)` | Minimum file size |
| `max` | `max(string\|int $size)` | Maximum file size |
| `between` | `between(string\|int $min, string\|int $max)` | File size range |
| `encoding` | `encoding(string $encoding)` | Required character encoding |
| `rules` | `rules(string\|array $rules)` | Merge additional rules |

**ImageFile-only:**

| Method | Signature | Description |
|--------|-----------|-------------|
| `dimensions` | `dimensions(Dimensions $dimensions)` | Pass a `Rule::dimensions()` instance |

```php
'document' => [File::types(['pdf', 'docx'])->max('10mb')],
'avatar' => [
    Rule::imageFile()
        ->max('5mb')
        ->dimensions(Rule::dimensions()->maxWidth(2000)->ratio(1)),
],
```

---

## Date

`Rule::date()` or `Rule::dateTime()`

| Method | Signature | Description |
|--------|-----------|-------------|
| `format` | `format(string $format)` | Validate against date format |
| `after` | `after(DateTimeInterface\|string $date)` | Must be after date |
| `afterOrEqual` | `afterOrEqual(DateTimeInterface\|string $date)` | Must be after or equal |
| `before` | `before(DateTimeInterface\|string $date)` | Must be before date |
| `beforeOrEqual` | `beforeOrEqual(DateTimeInterface\|string $date)` | Must be before or equal |
| `between` | `between(DateTimeInterface\|string $from, DateTimeInterface\|string $to)` | Must be between dates |
| `betweenOrEqual` | `betweenOrEqual(DateTimeInterface\|string $from, DateTimeInterface\|string $to)` | Between dates (inclusive) |
| `beforeToday` | `beforeToday()` | Must be before today |
| `afterToday` | `afterToday()` | Must be after today |
| `todayOrBefore` | `todayOrBefore()` | Today or earlier |
| `todayOrAfter` | `todayOrAfter()` | Today or later |
| `past` | `past()` | Must be in the past |
| `future` | `future()` | Must be in the future |
| `nowOrPast` | `nowOrPast()` | Now or earlier |
| `nowOrFuture` | `nowOrFuture()` | Now or later |

```php
'start_date' => [Rule::date()->afterToday()->format('Y-m-d')],
'end_date' => [Rule::date()->after('start_date')],
'birthday' => [Rule::date()->beforeToday()],
'expires_at' => [Rule::dateTime()->nowOrFuture()],
```

---

## Numeric

`Rule::numeric()`

| Method | Signature | Description |
|--------|-----------|-------------|
| `integer` | `integer()` | Must be an integer |
| `decimal` | `decimal(int $min, int $max = null)` | Specific decimal places |
| `min` | `min(int\|float $value)` | Minimum value |
| `max` | `max(int\|float $value)` | Maximum value |
| `between` | `between(int\|float $min, int\|float $max)` | Value in range |
| `exactly` | `exactly(int $value)` | Exact value (uses `size`) |
| `digits` | `digits(int $length)` | Exact digit count |
| `digitsBetween` | `digitsBetween(int $min, int $max)` | Digit count in range |
| `minDigits` | `minDigits(int $value)` | Minimum digit count |
| `maxDigits` | `maxDigits(int $value)` | Maximum digit count |
| `multipleOf` | `multipleOf(int\|float $value)` | Must be a multiple of value |
| `greaterThan` | `greaterThan(string $field)` | Greater than another field |
| `greaterThanOrEqualTo` | `greaterThanOrEqualTo(string $field)` | Greater than or equal |
| `lessThan` | `lessThan(string $field)` | Less than another field |
| `lessThanOrEqualTo` | `lessThanOrEqualTo(string $field)` | Less than or equal |
| `same` | `same(string $field)` | Must match another field |
| `different` | `different(string $field)` | Must differ from another field |

```php
'price' => [Rule::numeric()->decimal(2)->min(0)->max(99999.99)],
'quantity' => [Rule::numeric()->integer()->between(1, 100)],
'discount' => [Rule::numeric()->lessThan('price')],
```

---

## Email

`Rule::email()`

| Method | Signature | Description |
|--------|-----------|-------------|
| `rfcCompliant` | `rfcCompliant(bool $strict = false)` | RFC 5322 validation |
| `strict` | `strict()` | Strict RFC validation (alias for `rfcCompliant(true)`) |
| `validateMxRecord` | `validateMxRecord()` | Check DNS MX record |
| `preventSpoofing` | `preventSpoofing()` | Detect invalid unicode / homograph attacks |
| `withNativeValidation` | `withNativeValidation(bool $allowUnicode = false)` | Use PHP `filter_var` |

```php
'email' => [Rule::email()->rfcCompliant()->validateMxRecord()->preventSpoofing()],
```

---

## In & NotIn

`Rule::in(...)` and `Rule::notIn(...)`

Accept arrays, `Arrayable` instances, `UnitEnum` values, or strings.

```php
'role' => [Rule::in(['admin', 'editor', 'viewer'])],
'status' => [Rule::in(UserStatus::cases())],
'priority' => [Rule::notIn([Priority::Archived])],
```

---

## Conditional & Logic

### `Rule::when` / `Rule::unless`

Apply rules conditionally without splitting into if/else:

```php
'company' => [
    'string',
    Rule::when($request->type === 'business', ['required', 'max:255'], ['nullable']),
],
```

### `Rule::anyOf`

Value must satisfy at least one of the given rulesets:

```php
'identifier' => [Rule::anyOf([
    ['email'],
    ['integer', 'min:1'],
])],
```

### `Rule::can`

Gate/policy authorization as a validation rule:

```php
'post_id' => ['required', Rule::can('update', [Post::class, $this->route('post')])],
```

### `Rule::forEach`

Generate rules dynamically for each item in an array:

```php
'items.*' => [Rule::forEach(fn (mixed $value, string $attribute) => [
    'required',
    Rule::exists('products', 'id')->where('active', true),
])],
```
