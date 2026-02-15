# LazyCollection Reference

`Illuminate\Support\LazyCollection` uses PHP generators for constant-memory processing of large datasets.

## Creating LazyCollections

```php
use Illuminate\Support\LazyCollection;

// From generator
LazyCollection::make(function () {
    for ($i = 0; $i < 1000000; $i++) {
        yield $i;
    }
});

// From existing collection
$collection->lazy();

// Eloquent cursor (most common use case)
User::cursor(); // Returns LazyCollection

// From file
LazyCollection::make(function () {
    $handle = fopen('large-file.csv', 'r');
    while ($line = fgets($handle)) {
        yield str_getcsv($line);
    }
    fclose($handle);
});
```

## Key Differences from Collection

| Aspect | Collection | LazyCollection |
|--------|-----------|----------------|
| Memory | O(n) - all items in memory | O(1) - constant memory via generators |
| Evaluation | Eager - all methods execute immediately | Lazy - only evaluated when consumed |
| Mutating methods | Available (`push`, `pop`, `shift`, `splice`, `transform`, etc.) | Not available |
| Re-iteration | Free (data in memory) | Re-executes generator (use `remember()` to cache) |

## LazyCollection-Only Methods

### `takeUntilTimeout(DateTimeInterface $timeout)`

Enumerate values until a specified time, then stop. Useful for processing within time limits.

```php
LazyCollection::times(INF)
    ->takeUntilTimeout(now()->addMinutes(5))
    ->each(fn($item) => processItem($item));
```

### `tapEach(callable $callback)`

Like `each()` but deferred - callback runs only when items are actually pulled from the collection.

```php
$lazy = LazyCollection::times(INF)
    ->tapEach(fn($value) => dump($value)); // Nothing happens yet

$lazy->take(3)->all(); // Now dumps 1, 2, 3
```

### `throttle(int $seconds)`

Wait a number of seconds between yielding each value. Useful for rate-limited API calls.

```php
User::cursor()
    ->throttle(1) // 1 second between each user
    ->each(fn($user) => callExternalApi($user));
```

### `remember()`

Cache already-enumerated values so re-iterating doesn't re-execute the generator.

```php
$lazy = LazyCollection::make(function () {
    yield 1;
    yield 2;
})->remember();

$lazy->all(); // Executes generator
$lazy->all(); // Uses cached values
```

### `withHeartbeat(int $seconds, callable $callback)`

Execute a callback at regular intervals during enumeration. Useful for extending locks or sending progress updates.

```php
$lazy->withHeartbeat(30, function () use ($lock) {
    $lock->extend(60);
})->each(fn($item) => process($item));
```

## When to Use LazyCollection

Use **LazyCollection** when:
- Processing 10,000+ database records (use `Model::cursor()`)
- Reading large files line-by-line
- Working with infinite sequences
- Calling rate-limited external APIs (with `throttle()`)
- Processing within time budgets (with `takeUntilTimeout()`)

Use **Collection** when:
- Dataset fits comfortably in memory (< 10,000 items typically)
- Need random access or mutating methods
- Need to iterate multiple times without caching
- Need methods not available on LazyCollection
