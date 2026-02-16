# Laravel Validation Rules Reference

Complete reference for all built-in validation rules (Laravel 12.x). Rules are categorized by purpose.

## Table of Contents

- [Presence & Optionality](#presence--optionality)
- [Type Constraints](#type-constraints)
- [String Rules](#string-rules)
- [Numeric Rules](#numeric-rules)
- [Date Rules](#date-rules)
- [Array & List Rules](#array--list-rules)
- [File & Image Rules](#file--image-rules)
- [Database Rules](#database-rules)
- [Comparison Rules](#comparison-rules)
- [Conditional Rules](#conditional-rules)
- [Exclusion Rules](#exclusion-rules)
- [Prohibition Rules](#prohibition-rules)
- [Flow Control](#flow-control)

---

## Presence & Optionality

| Rule | Signature | Description |
|------|-----------|-------------|
| `required` | `required` | Must be present and not empty |
| `required_if` | `required_if:field,val1,val2,...` | Required when `field` equals any of the values |
| `required_if_accepted` | `required_if_accepted:field` | Required when `field` is accepted (truthy) |
| `required_if_declined` | `required_if_declined:field` | Required when `field` is declined (falsy) |
| `required_unless` | `required_unless:field,val1,...` | Required unless `field` equals one of the values |
| `required_with` | `required_with:field1,field2,...` | Required if **any** of the listed fields are present |
| `required_with_all` | `required_with_all:field1,field2,...` | Required if **all** of the listed fields are present |
| `required_without` | `required_without:field1,...` | Required if **any** of the listed fields are absent |
| `required_without_all` | `required_without_all:field1,...` | Required if **all** of the listed fields are absent |
| `sometimes` | `sometimes` | Only validate if the field is present in input |
| `nullable` | `nullable` | Allow `null` as a valid value |
| `present` | `present` | Must exist in input data (even if empty) |
| `present_if` | `present_if:field,val,...` | Must be present when `field` matches value |
| `present_unless` | `present_unless:field,val` | Must be present unless `field` matches value |
| `present_with` | `present_with:field1,...` | Must be present if any listed fields are present |
| `present_with_all` | `present_with_all:field1,...` | Must be present if all listed fields are present |
| `filled` | `filled` | Must not be empty when present |
| `missing` | `missing` | Must not be present in input |
| `missing_if` | `missing_if:field,val,...` | Must be missing when `field` matches any value |
| `missing_unless` | `missing_unless:field,val` | Must be missing unless `field` matches value |
| `missing_with` | `missing_with:field1,...` | Must be missing if any listed fields are present |
| `missing_with_all` | `missing_with_all:field1,...` | Must be missing if all listed fields are present |

---

## Type Constraints

| Rule | Signature | Description |
|------|-----------|-------------|
| `string` | `string` | Must be a string |
| `integer` | `integer[:strict]` | Must be an integer. `strict` checks PHP type |
| `numeric` | `numeric[:strict]` | Must be numeric. `strict` checks PHP int/float type |
| `boolean` | `boolean[:strict]` | Acceptable: `true`, `false`, `1`, `0`, `"1"`, `"0"`. `strict` for true/false only |
| `array` | `array[:key1,key2,...]` | Must be array. Keys param restricts allowed keys |
| `list` | `list` | Must be a zero-indexed sequential array |
| `json` | `json` | Must be a valid JSON string |
| `file` | `file` | Must be a successfully uploaded file |
| `image` | `image[:allow_svg]` | Must be an image (jpeg, png, bmp, gif, webp, svg optional) |
| `date` | `date` | Must be a valid, non-relative date via `strtotime` |

---

## String Rules

| Rule | Signature | Description |
|------|-----------|-------------|
| `alpha` | `alpha[:ascii]` | Unicode letters only. `ascii` restricts to a-zA-Z |
| `alpha_dash` | `alpha_dash[:ascii]` | Letters, numbers, dashes, underscores |
| `alpha_num` | `alpha_num[:ascii]` | Letters and numbers only |
| `ascii` | `ascii` | 7-bit ASCII characters only |
| `email` | `email[:rfc,strict,dns,spoof,filter,filter_unicode]` | Valid email address with optional validators |
| `url` | `url[:http,https,...]` | Valid URL. Optional protocol restriction |
| `active_url` | `active_url` | Has a valid DNS A or AAAA record |
| `ip` | `ip` | Valid IP address |
| `ipv4` | `ipv4` | Valid IPv4 address |
| `ipv6` | `ipv6` | Valid IPv6 address |
| `mac_address` | `mac_address` | Valid MAC address |
| `uuid` | `uuid[:version]` | Valid UUID. Optional version (e.g., `uuid:4`) |
| `ulid` | `ulid` | Valid ULID |
| `hex_color` | `hex_color` | Valid hexadecimal color code |
| `lowercase` | `lowercase` | Must be entirely lowercase |
| `uppercase` | `uppercase` | Must be entirely uppercase |
| `starts_with` | `starts_with:val1,val2,...` | Must start with one of the given values |
| `doesnt_start_with` | `doesnt_start_with:val1,...` | Must not start with given values |
| `ends_with` | `ends_with:val1,val2,...` | Must end with one of the given values |
| `doesnt_end_with` | `doesnt_end_with:val1,...` | Must not end with given values |
| `regex` | `regex:pattern` | Must match the given regex (use array syntax to avoid `\|` issues) |
| `not_regex` | `not_regex:pattern` | Must not match the given regex |
| `confirmed` | `confirmed[:field]` | Must match `{field}_confirmation` or custom field |
| `timezone` | `timezone[:group,country]` | Valid timezone (e.g., `timezone:Africa`) |

---

## Numeric Rules

| Rule | Signature | Description |
|------|-----------|-------------|
| `min` | `min:value` | Minimum: chars for strings, value for numbers, count for arrays, KB for files |
| `max` | `max:value` | Maximum (same context-dependent sizing as `min`) |
| `size` | `size:value` | Exact size (same context-dependent sizing) |
| `between` | `between:min,max` | Inclusive range (same sizing) |
| `digits` | `digits:value` | Must be an integer with exactly `value` digits |
| `digits_between` | `digits_between:min,max` | Integer with digit count in range |
| `min_digits` | `min_digits:value` | Integer with at least `value` digits |
| `max_digits` | `max_digits:value` | Integer with at most `value` digits |
| `decimal` | `decimal:min[,max]` | Must be numeric with specified decimal places |
| `multiple_of` | `multiple_of:value` | Must be a multiple of `value` |
| `gt` | `gt:field` | Greater than another field's value |
| `gte` | `gte:field` | Greater than or equal to another field |
| `lt` | `lt:field` | Less than another field |
| `lte` | `lte:field` | Less than or equal to another field |
| `same` | `same:field` | Must match given field's value |
| `different` | `different:field` | Must differ from given field's value |

---

## Date Rules

| Rule | Signature | Description |
|------|-----------|-------------|
| `date` | `date` | Valid date via `strtotime` |
| `date_format` | `date_format:format1,format2,...` | Must match one of the given PHP date formats |
| `date_equals` | `date_equals:date` | Must equal given date |
| `after` | `after:date_or_field` | Must be after a date (literal or field reference) |
| `after_or_equal` | `after_or_equal:date_or_field` | Must be after or equal to date |
| `before` | `before:date_or_field` | Must be before a date |
| `before_or_equal` | `before_or_equal:date_or_field` | Must be before or equal to date |

**Note:** Date parameters can be a `strtotime`-compatible string, a PHP date format result, or another field name. Use field names to compare two input dates (e.g., `after:start_date`).

---

## Array & List Rules

| Rule | Signature | Description |
|------|-----------|-------------|
| `array` | `array[:key1,key2,...]` | Must be array. Keys restrict allowed keys |
| `list` | `list` | Must be zero-indexed sequential array |
| `in` | `in:val1,val2,...` or `Rule::in([...])` | Must be one of listed values |
| `not_in` | `not_in:val1,...` or `Rule::notIn([...])` | Must not be one of listed values |
| `in_array` | `in_array:other_field.*` | Value must exist in other field's array |
| `distinct` | `distinct[:strict\|ignore_case]` | No duplicate values in array field |
| `required_array_keys` | `required_array_keys:key1,key2,...` | Array must contain specified keys |
| `contains` | `Rule::contains([val1, val2])` | Array must contain all given values |
| `doesnt_contain` | `Rule::doesntContain([val1])` | Array must not contain given values |

**Wildcard notation for nested arrays:**

```php
'users' => 'required|array',
'users.*.name' => 'required|string|max:255',
'users.*.email' => 'required|email',
'users.*.roles' => 'array',
'users.*.roles.*' => 'exists:roles,name',
```

---

## File & Image Rules

| Rule | Signature | Description |
|------|-----------|-------------|
| `file` | `file` | Must be uploaded file |
| `image` | `image[:allow_svg]` | Must be image (jpeg, png, bmp, gif, webp) |
| `mimes` | `mimes:ext1,ext2,...` | File MIME type by extension (e.g., `mimes:jpg,png`) |
| `mimetypes` | `mimetypes:type1,...` | Exact MIME type (e.g., `mimetypes:video/mp4`) |
| `extensions` | `extensions:ext1,ext2,...` | File extension check. Combine with `mimes` for security |
| `dimensions` | `dimensions:constraints` | Image dimensions (see below) |
| `encoding` | `encoding:charset` | Must match character encoding |

**Dimensions constraints:** `min_width`, `max_width`, `min_height`, `max_height`, `width`, `height`, `ratio`

```php
'avatar' => 'dimensions:min_width=100,min_height=100,ratio=1',
// Or with Rule builder:
'avatar' => [Rule::dimensions()->maxWidth(1000)->ratio(3/2)],
```

**File rule builder:**

```php
use Illuminate\Validation\Rules\File;

'doc' => [File::types(['pdf', 'docx'])->min('1kb')->max('10mb')],
'photo' => [File::image()->dimensions(Rule::dimensions()->maxWidth(2000))],
```

---

## Database Rules

### `exists`

```php
// Basic
'department_id' => 'exists:departments,id',

// With Rule builder (add query constraints)
'department_id' => [Rule::exists('departments', 'id')->where('active', true)],

// Specify connection
'id' => 'exists:mysql.users,id',
```

### `unique`

```php
// Basic
'email' => 'unique:users,email',

// Ignore current record (for updates)
'email' => [Rule::unique('users', 'email')->ignore($user->id)],

// Ignore soft-deleted records
'email' => [Rule::unique('users', 'email')->withoutTrashed()],

// With extra where clauses
'email' => [Rule::unique('users')->where('account_id', $accountId)],
```

---

## Comparison Rules

| Rule | Signature | Description |
|------|-----------|-------------|
| `accepted` | `accepted` | Must be "yes", "on", 1, "1", true, "true" |
| `accepted_if` | `accepted_if:field,val,...` | Accepted when another field matches value |
| `declined` | `declined` | Must be "no", "off", 0, "0", false, "false" |
| `declined_if` | `declined_if:field,val,...` | Declined when another field matches value |
| `same` | `same:field` | Must match given field |
| `different` | `different:field` | Must differ from given field |
| `gt` | `gt:field` | Greater than another field |
| `gte` | `gte:field` | Greater than or equal |
| `lt` | `lt:field` | Less than another field |
| `lte` | `lte:field` | Less than or equal |

---

## Conditional Rules

| Rule | Signature | Description |
|------|-----------|-------------|
| `required_if` | `required_if:field,val,...` | Required when field matches |
| `required_if_accepted` | `required_if_accepted:field` | Required when field is truthy |
| `required_if_declined` | `required_if_declined:field` | Required when field is falsy |
| `required_unless` | `required_unless:field,val,...` | Required unless field matches |
| `required_with` | `required_with:field1,...` | Required if any fields present |
| `required_with_all` | `required_with_all:field1,...` | Required if all fields present |
| `required_without` | `required_without:field1,...` | Required if any fields absent |
| `required_without_all` | `required_without_all:field1,...` | Required if all fields absent |

**Fluent conditional rules:**

```php
Rule::requiredIf(true)
Rule::requiredIf(fn () => $user->isPremium())
Rule::excludeIf(fn () => $request->type === 'free')
Rule::prohibitedIf(fn () => $user->isGuest())
```

---

## Exclusion Rules

| Rule | Signature | Description |
|------|-----------|-------------|
| `exclude` | `exclude` | Always exclude from validated data |
| `exclude_if` | `exclude_if:field,val` | Exclude when field matches value |
| `exclude_unless` | `exclude_unless:field,val` | Exclude unless field matches |
| `exclude_with` | `exclude_with:field` | Exclude when field is present |
| `exclude_without` | `exclude_without:field` | Exclude when field is absent |

---

## Prohibition Rules

| Rule | Signature | Description |
|------|-----------|-------------|
| `prohibited` | `prohibited` | Must be missing or empty |
| `prohibited_if` | `prohibited_if:field,val,...` | Prohibited when field matches |
| `prohibited_if_accepted` | `prohibited_if_accepted:field` | Prohibited when field is truthy |
| `prohibited_if_declined` | `prohibited_if_declined:field` | Prohibited when field is falsy |
| `prohibited_unless` | `prohibited_unless:field,val,...` | Prohibited unless field matches |
| `prohibits` | `prohibits:field1,...` | If present, listed fields must be absent |

---

## Flow Control

| Rule | Signature | Description |
|------|-----------|-------------|
| `bail` | `bail` | Stop running rules for this field after first failure. **Must be first rule** |
| `sometimes` | `sometimes` | Only validate if field exists in input |
| `nullable` | `nullable` | Allow null values |

**`Rule::anyOf`** — value must pass at least one of the given rulesets:

```php
'identifier' => [Rule::anyOf([
    ['email'],
    ['integer', 'min:1'],
])],
```
