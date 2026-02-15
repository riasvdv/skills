# Laravel Collection Methods Reference

Complete reference for `Illuminate\Support\Collection` (Laravel 12.x). Methods are categorized by purpose.

## Table of Contents

- [Creating Collections](#creating-collections)
- [Accessing Items](#accessing-items)
- [Existence Checks](#existence-checks)
- [Filtering](#filtering)
- [Transformation](#transformation)
- [Sorting](#sorting)
- [Grouping & Partitioning](#grouping--partitioning)
- [Aggregation & Math](#aggregation--math)
- [Combining & Merging](#combining--merging)
- [Diffing & Intersecting](#diffing--intersecting)
- [Iteration & Side-Effects](#iteration--side-effects)
- [String Operations](#string-operations)
- [Reshaping & Slicing](#reshaping--slicing)
- [Conditional Execution](#conditional-execution)
- [Piping & Tapping](#piping--tapping)
- [Serialization & Output](#serialization--output)
- [Mutating Methods](#mutating-methods)
- [Static Methods](#static-methods)

---

## Creating Collections

```php
// From array
$c = collect([1, 2, 3]);

// Static constructors
Collection::make([1, 2, 3]);
Collection::fromJson('{"name":"Taylor"}');
Collection::range(1, 10);           // [1, 2, ..., 10]
Collection::times(5, fn($n) => $n * 2); // [2, 4, 6, 8, 10]
Collection::wrap($value);           // Wraps non-collection in collection
Collection::unwrap($value);         // Unwraps collection to array

// Convert LazyCollection to Collection
$lazy->collect();
```

## Accessing Items

| Method | Description |
|--------|-------------|
| `all()` | Returns underlying array |
| `get($key, $default)` | Item at key, with optional default (value or closure) |
| `first($callback, $default)` | First item (optionally matching callback) |
| `firstOrFail($callback)` | Like `first()` but throws `ItemNotFoundException` |
| `firstWhere($key, $op, $value)` | First item where key matches condition |
| `last($callback, $default)` | Last item (optionally matching callback) |
| `sole($callback)` | Single matching item; throws if 0 or 2+ match |
| `value($key, $default)` | Value of `$key` from first item |
| `after($value, strict: false)` | Item after the given value |
| `before($value, strict: false)` | Item before the given value |
| `random($count)` | Random item(s) |
| `search($value, $strict)` | Key of given value (returns `false` if not found) |

## Existence Checks

| Method | Description |
|--------|-------------|
| `has($key)` | Key exists in collection |
| `hasAny($keys)` | Any of the given keys exist |
| `hasMany($keys)` | More than one of the keys exists |
| `hasSole($keys)` | Exactly one of the keys exists |
| `contains($key, $op, $value)` | Value exists (loose comparison). Also accepts closure |
| `containsStrict($key, $value)` | Value exists (strict comparison) |
| `doesntContain($key, $op, $value)` | Inverse of `contains` |
| `doesntContainStrict(...)` | Inverse of `containsStrict` |
| `some(...)` | Alias for `contains` |
| `every($callback)` | All items pass truth test (empty returns true) |
| `isEmpty()` | Collection is empty |
| `isNotEmpty()` | Collection is not empty |

**`has` vs `contains`**: `has` checks for key existence, `contains` checks for value existence.

## Filtering

| Method | Description |
|--------|-------------|
| `filter($callback)` | Keep items passing test. No callback = remove falsy |
| `reject($callback)` | Inverse of `filter` |
| `where($key, $op, $value)` | Filter by key/value with operator (`=`, `>`, `<`, etc.) |
| `whereStrict($key, $value)` | `where` with strict comparison |
| `whereBetween($key, [$min, $max])` | Items where key is between range |
| `whereNotBetween($key, [$min, $max])` | Inverse of `whereBetween` |
| `whereIn($key, $values)` | Items where key is in array |
| `whereInStrict($key, $values)` | Strict version |
| `whereNotIn($key, $values)` | Items where key is NOT in array |
| `whereNotInStrict($key, $values)` | Strict version |
| `whereNull($key)` | Items where key is null |
| `whereNotNull($key)` | Items where key is not null |
| `whereInstanceOf($type)` | Items that are instances of class |
| `only($keys)` | Only items with specified keys |
| `except($keys)` | All items except specified keys |
| `unique($key, $strict)` | Unique items (by key or callback) |
| `uniqueStrict($key)` | Strict version |
| `duplicates($key, $strict)` | Returns duplicate values |
| `duplicatesStrict($key)` | Strict version |
| `nth($step, $offset)` | Every n-th element |

## Transformation

| Method | Description |
|--------|-------------|
| `map($callback)` | Apply callback to each item, return new collection |
| `mapInto($class)` | Map items into new class instances |
| `mapSpread($callback)` | Map nested items by spreading into callback args |
| `mapWithKeys($callback)` | Map returning `[$key => $value]` pairs |
| `mapToGroups($callback)` | Map returning `[$key => $value]`, grouping by key |
| `flatMap($callback)` | Map then flatten one level |
| `pluck($value, $key)` | Extract column of values, optionally keyed |
| `select($keys)` | Select specific keys from each item (like SQL SELECT) |
| `flatten($depth)` | Flatten multi-dimensional collection |
| `collapse()` | Collapse collection of arrays into single flat collection |
| `collapseWithKeys()` | Collapse preserving keys |
| `flip()` | Swap keys and values |
| `keys()` | All keys |
| `values()` | All values, re-indexed numerically |
| `dot()` | Flatten to dot-notation keys |
| `undot()` | Expand dot-notation keys to nested structure |
| `ensure($type)` | Verify all items are of given type(s) |
| `multiply($count)` | Repeat collection items n times |

## Sorting

| Method | Description |
|--------|-------------|
| `sort($callback)` | Sort values (preserves keys) |
| `sortDesc()` | Sort values descending |
| `sortBy($key, $desc, $options)` | Sort by key or callback |
| `sortByDesc($key, $options)` | Sort by key descending |
| `sortKeys($options)` | Sort by keys |
| `sortKeysDesc($options)` | Sort by keys descending |
| `sortKeysUsing($callback)` | Sort keys using custom comparison |
| `reverse()` | Reverse order |
| `shuffle()` | Randomly shuffle |

## Grouping & Partitioning

| Method | Description |
|--------|-------------|
| `groupBy($key, $preserveKeys)` | Group items by key or callback. Returns `Collection<Collection>` |
| `keyBy($key)` | Key collection by field (last wins on duplicates) |
| `partition($callback)` | Split into `[$passing, $failing]` |
| `chunk($size)` | Split into chunks of given size |
| `chunkWhile($callback)` | Split into chunks while callback returns true |
| `split($count)` | Split into given number of groups |
| `splitIn($count)` | Split into given number of groups (final group gets remainder) |
| `sliding($size, $step)` | Sliding window of given size |
| `forPage($page, $perPage)` | Items for given page number |

**`groupBy` vs `mapToGroups`**: `groupBy` groups by an existing key. `mapToGroups` lets the callback define both key and value for grouping.

**`keyBy` vs `groupBy`**: `keyBy` results in one item per key (last wins). `groupBy` collects all items with that key into an array.

## Aggregation & Math

| Method | Description |
|--------|-------------|
| `count()` | Number of items |
| `countBy($callback)` | Count occurrences (by value or callback result) |
| `sum($key)` | Sum of values |
| `avg($key)` | Average value |
| `average($key)` | Alias for `avg` |
| `min($key)` | Minimum value |
| `max($key)` | Maximum value |
| `median($key)` | Median value |
| `mode($key)` | Mode (most frequent value) |
| `percentage($callback, $precision)` | Percentage of items passing truth test |
| `reduce($callback, $initial)` | Reduce to single value |
| `reduceSpread($callback, ...$initial)` | Reduce with multiple accumulators |

## Combining & Merging

| Method | Description |
|--------|-------------|
| `merge($items)` | Merge items (string keys overwrite, numeric keys append) |
| `mergeRecursive($items)` | Merge recursively (values become arrays on conflict) |
| `concat($items)` | Append values (always re-indexes numerically) |
| `combine($values)` | Use collection as keys, argument as values |
| `union($items)` | Union (existing keys preserved) |
| `replace($items)` | Replace matching keys |
| `replaceRecursive($items)` | Replace recursively |
| `zip($items)` | Zip collection with arrays (pair items by index) |
| `crossJoin(...$lists)` | Cartesian product with given arrays |
| `pad($size, $value)` | Pad collection to given size |

## Diffing & Intersecting

| Method | Description |
|--------|-------------|
| `diff($items)` | Values not in given items |
| `diffAssoc($items)` | Key/value pairs not matching |
| `diffAssocUsing($items, $callback)` | `diffAssoc` with custom key comparison |
| `diffKeys($items)` | Keys not in given items |
| `intersect($items)` | Values present in both |
| `intersectUsing($items, $callback)` | `intersect` with custom comparison |
| `intersectAssoc($items)` | Key/value pairs matching both |
| `intersectAssocUsing($items, $callback)` | `intersectAssoc` with custom key comparison |
| `intersectByKeys($items)` | Keys present in both |

## Iteration & Side-Effects

| Method | Description |
|--------|-------------|
| `each($callback)` | Iterate items (return `false` to break). Returns original collection |
| `eachSpread($callback)` | `each` with nested items spread as arguments |

## String Operations

| Method | Description |
|--------|-------------|
| `implode($key, $glue)` | Join values with glue string |
| `join($glue, $finalGlue)` | Join with final separator (e.g., `'a, b, and c'`) |

## Reshaping & Slicing

| Method | Description |
|--------|-------------|
| `take($count)` | First n items (negative = last n) |
| `takeUntil($value)` | Items until value/callback matches |
| `takeWhile($value)` | Items while value/callback matches |
| `skip($count)` | Skip first n items |
| `skipUntil($value)` | Skip until value/callback matches |
| `skipWhile($value)` | Skip while value/callback matches |
| `slice($offset, $length)` | Slice from offset |

## Conditional Execution

| Method | Description |
|--------|-------------|
| `when($condition, $callback, $default)` | Execute callback if condition is truthy |
| `whenEmpty($callback, $default)` | Execute callback if collection is empty |
| `whenNotEmpty($callback, $default)` | Execute callback if collection is not empty |
| `unless($condition, $callback, $default)` | Execute callback if condition is falsy |
| `unlessEmpty($callback, $default)` | Alias for `whenNotEmpty` |
| `unlessNotEmpty($callback, $default)` | Alias for `whenEmpty` |

## Piping & Tapping

| Method | Description |
|--------|-------------|
| `pipe($callback)` | Pass entire collection to callback, return result |
| `pipeInto($class)` | Pass collection into class constructor |
| `pipeThrough($pipes)` | Pass collection through array of callables |
| `tap($callback)` | Pass collection to callback but return original collection |

## Serialization & Output

| Method | Description |
|--------|-------------|
| `toArray()` | Convert to plain array (recursively) |
| `toJson($options)` | Convert to JSON string |
| `toPrettyJson($options)` | Convert to pretty-printed JSON |
| `dump()` | Dump items (continues execution) |
| `dd()` | Dump items and die |

## Mutating Methods

These methods modify the collection **in place** instead of returning a new one. Not available on `LazyCollection`.

| Method | Description |
|--------|-------------|
| `transform($callback)` | Apply callback to each item, modifying original |
| `push(...$values)` | Append value(s) to end |
| `prepend($value, $key)` | Add item to beginning |
| `put($key, $value)` | Set key/value pair |
| `pull($key, $default)` | Remove and return item by key |
| `pop($count)` | Remove and return last item(s) |
| `shift($count)` | Remove and return first item(s) |
| `splice($offset, $length, $replacement)` | Remove and/or replace slice |
| `forget($keys)` | Remove item(s) by key |

## Static Methods

| Method | Description |
|--------|-------------|
| `Collection::make($items)` | Create new collection |
| `Collection::fromJson($json, $flags)` | Create from JSON string |
| `Collection::range($from, $to)` | Create from numeric range |
| `Collection::times($count, $callback)` | Create by invoking callback n times |
| `Collection::wrap($value)` | Wrap value in collection |
| `Collection::unwrap($value)` | Unwrap collection to array |
| `Collection::macro($name, $macro)` | Register custom method |
