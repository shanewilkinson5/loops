# Loops

A comprehensive list of loop constructs across programming languages.

Each entry shows the loop forms the language actually provides — counted loops,
conditional loops, iteration over collections, and whatever the language calls
its post-test loop (if it has one). This guide explores the semantics, use cases,
and design philosophy behind different loop constructs.

## Contents

- [C](#c)
- [C++](#c-1)
- [Java](#java)
- [C#](#c-2)
- [Python](#python)
- [JavaScript](#javascript)
- [Go](#go)
- [Rust](#rust)
- [Ruby](#ruby)
- [Bash](#bash)
- [Lua](#lua)
- [Haskell](#haskell)
- [SQL](#sql)
- [TypeScript](#typescript)
- [Kotlin](#kotlin)
- [Swift](#swift)
- [PHP](#php)
- [Perl](#perl)
- [Clojure](#clojure)
- [F#](#f)
- [Scala](#scala)

---

## C

```c
for (int i = 0; i < 10; i++) { }        /* counted loop */
while (cond) { }                         /* pre-test conditional */
do { } while (cond);                     /* post-test conditional */
goto top;                                /* unstructured jump */
```

**Characteristics:**
- No built-in collection iteration — you must manually index arrays or walk pointers
- The `for` loop is the workhorse: initializer, condition, and increment all in one place
- `do-while` guarantees at least one execution, useful for input validation
- C is the foundation of most imperative loop constructs; most languages either inherit or react against C's style

**Use Cases:**
- **Counted loops**: Fixed iterations over arrays or ranges
- **Pointer-based iteration**: Walking linked lists or manual memory structures
- **Complex initialization/increment**: When simple counter patterns don't suffice

---

## C++

```cpp
for (int i = 0; i < 10; ++i) { }         // counted loop
for (auto& x : container) { }            // range-based (C++11)
while (cond) { }                         // pre-test conditional
do { } while (cond);                     // post-test conditional
std::for_each(v.begin(), v.end(), fn);   // algorithm library
std::ranges::for_each(v, fn);            // ranges library (C++20)

// Iterator-based loops (lower-level)
for (auto it = container.begin(); it != container.end(); ++it) { }

// Structured bindings (C++17)
for (auto [key, value] : map) { }
```

**Characteristics:**
- Range-based `for` (C++11) brings Python-like simplicity without sacrificing performance
- The algorithm library (`<algorithm>`) offers functional alternatives to imperative loops
- Multiple layers of abstraction: raw iterators, ranges, and functional combinators
- Performance-conscious: range-based loops can optimize away bounds checks when appropriate

**Use Cases:**
- **Generic containers**: Works with any type that defines iterators or ranges
- **Performance-critical code**: Inline optimizations for range-based loops
- **Functional style**: `std::for_each` and ranges for declarative iteration

---

## Java

```java
for (int i = 0; i < 10; i++) { }         // counted loop
for (String s : list) { }                // enhanced for (Java 5)
while (cond) { }                         // pre-test conditional
do { } while (cond);                     // post-test conditional
list.forEach(s -> { });                  // Iterable.forEach (Java 8)
IntStream.range(0, 10).forEach(i -> {}); // streams (Java 8)
```

**Characteristics:**
- Enhanced `for` (introduced in Java 5) works on anything implementing `Iterable`
- Streams offer lazy evaluation and functional composition (Java 8+)
- No collection iteration before Java 5 — the enhanced `for` was a major usability improvement
- Streams can be parallel: `list.parallelStream().forEach(...)`

**Use Cases:**
- **Collections and arrays**: Enhanced `for` is the standard choice
- **Transformative operations**: Streams for filtering, mapping, reducing
- **Parallel processing**: Streams with `parallelStream()`

---

## C#

```csharp
for (int i = 0; i < 10; i++) { }        // counted loop
foreach (var x in collection) { }       // iterates over IEnumerable
while (cond) { }                        // pre-test conditional
do { } while (cond);                    // post-test conditional

// LINQ (Language Integrated Query)
collection.ForEach(x => { });
from x in collection select x
    .Where(x => x > 5)
    .Select(x => x * 2);

// Iterator methods (yield return)
IEnumerable<int> RangeIterator(int start, int end) {
    for (int i = start; i <= end; i++) yield return i;
}
foreach (var x in RangeIterator(1, 10)) { }
```

**Characteristics:**
- `foreach` works on anything implementing `IEnumerable` or `IAsyncEnumerable`
- LINQ provides SQL-like syntax for querying and transforming collections
- Iterator methods with `yield return` let you create custom iteration patterns
- First-class async iteration with `await foreach` (C# 8.0)

**Use Cases:**
- **Collections**: `foreach` is the idiomatic choice for all enumerable types
- **Queries**: LINQ for complex filtering, projection, and aggregation
- **Async iteration**: `await foreach` for async streams

---

## Python

```python
for x in iterable: ...                  # iteration (the only loop keyword for sequences)
for i in range(10): ...                 # counted, but via the range() function
while cond: ...                         # pre-test conditional

# Loop-else: runs if loop completes without break
for x in xs:
    if x == target:
        break
else:
    print("not found")

# Comprehensions (implicit loops)
[x for x in xs]                         # list comprehension
{x for x in xs}                         # set comprehension
{k: v for k, v in items}                # dict comprehension
(x for x in xs)                         # generator expression

# Enumerate and zip
for i, x in enumerate(xs): ...
for a, b in zip(xs, ys): ...
```

**Characteristics:**
- No `do-while` or C-style three-clause `for` — Python favors explicit iterator protocols
- `for-else` is unique: the `else` block runs if the loop completes normally (without `break`)
- Comprehensions are syntactic sugar for implicit loops, optimized at the bytecode level
- Generators offer lazy evaluation: `(x for x in range(1000000))` doesn't allocate upfront
- `enumerate()` and `zip()` handle common patterns elegantly

**Use Cases:**
- **Collections**: `for x in iterable` is the only loop form you typically need
- **Counted loops**: `range()` provides counts without C-style syntax
- **Transformation**: Comprehensions are faster and more Pythonic than explicit loops
- **Lazy evaluation**: Generators for memory-efficient processing of large datasets

---

## JavaScript

```javascript
for (let i = 0; i < 10; i++) { }        // counted loop
for (const x of iterable) { }           // iteration protocol (values)
for (const k in obj) { }                // enumeration (keys, including inherited)
while (cond) { }                        // pre-test conditional
do { } while (cond);                    // post-test conditional
arr.forEach(x => { });                  // array method (cannot break)
arr.map(x => x * 2);                    // transformation (returns new array)

// Async iteration
for await (const x of asyncIterable) { } // async/await iteration

// Promises and async
Promise.all(promises).then(results => {});

// Older: function recursion for iteration
function iterate(arr, i) {
    if (i >= arr.length) return;
    console.log(arr[i]);
    iterate(arr, i + 1);
}
```

**Characteristics:**
- `for...of` (ES6) provides iterator-based loops; `for...in` enumerates properties (including inherited)
- `.forEach()` is a method, not a keyword, and cannot be exited early with `break`
- `for await...of` handles async iteration (ES2018)
- The language's event-driven nature means loops often involve callbacks or promises
- `map()`, `filter()`, `reduce()` provide functional alternatives

**Use Cases:**
- **Collections**: `for...of` for modern browsers/Node.js
- **Object properties**: `for...in` for dynamic inspection (though `Object.entries()` is often better)
- **Async workflows**: Async iteration and promise chains
- **Functional style**: `map()`, `filter()`, `reduce()` for declarative transformations

---

## Go

```go
for i := 0; i < 10; i++ { }             // counted loop
for cond { }                            // while-style (pre-test)
for { }                                 // infinite loop (break to exit)
for i, v := range slice { }             // range over slice
for i, v := range array { }             // range over array
for k, v := range map { }               // range over map
for i := range 10 { }                   // range-over-int (Go 1.22)
for _, v := range slice { }             // skip index with blank identifier
```

**Characteristics:**
- One keyword (`for`), multiple forms — simplicity is a design goal
- No `while` keyword; `for cond { }` is the pre-test conditional
- `range` unifies iteration over arrays, slices, maps, strings, and channels
- Range-over-int (Go 1.22) lets you write `for i := range 10` instead of `for i := 0; i < 10; i++`
- The blank identifier `_` skips unwanted loop variables

**Use Cases:**
- **Any iteration**: One keyword, multiple patterns — consistency and simplicity
- **Channels**: `for x := range ch` receives until channel closes
- **Efficiency**: Range iteration is optimized for each container type

---

## Rust

```rust
for x in iterable { }                   // consumes an IntoIterator
for x in &iterable { }                  // borrows (immutable)
for x in &mut iterable { }              // borrows (mutable)
while cond { }                          // pre-test conditional
while let Some(x) = it.next() { }       // pattern matching on iterator
loop { }                                // infinite loop (expression form)
let x = loop {
    break 5;                            // break with a value
};
```

**Characteristics:**
- No `do-while` — Rust's design philosophy favors explicitness
- `for` works with ownership: `for x in v` consumes `v`, `for x in &v` borrows it
- `while let` combines pattern matching with loops, common for iterator exhaustion
- `loop` is an expression that can return a value via `break`
- Iterator adapters (`.map()`, `.filter()`, `.fold()`) provide functional alternatives

**Use Cases:**
- **Collections**: Iteration respects ownership rules and prevents use-after-free
- **Pattern matching**: `while let` for graceful termination of iterators
- **Computed loops**: `loop` with `break` as an expression
- **Performance**: Zero-cost abstractions; iterator chains compile to efficient code

---

## Ruby

```ruby
5.times { |i| }                         // repeat N times
(1..10).each { |i| }                    // closed range iteration
(1...10).each { |i| }                   // open range iteration (excludes end)
collection.each { |x| }                 // collection iteration
collection.each_with_index { |x, i| }   // iteration with index
while cond; end                         // pre-test conditional
until cond; end                         // negated pre-test conditional
loop { break if done }                  // infinite with explicit break
begin; end while cond                   // post-test conditional
begin; end until cond                   // post-test negated conditional

# Functional alternatives
[1, 2, 3].map { |x| x * 2 }             // transformation
[1, 2, 3].select { |x| x > 1 }          // filtering
[1, 2, 3].reduce(:+)                    // aggregation
```

**Characteristics:**
- Blocks (`{ |x| ... }`) are the primary iteration mechanism
- `.times`, `.each`, `.map` are methods that take blocks, not keywords
- `until` is the negation of `while`, providing semantic clarity
- `loop { break if cond }` is idiomatic for infinite loops with early exit
- Method names are often verb-like: `.each`, `.map`, `.select`, `.reduce`

**Use Cases:**
- **Blocks**: The fundamental abstraction for iteration and callbacks
- **Ranges and collections**: Unified via the `.each` method
- **Functional transformations**: `map`, `select`, `reduce` for data manipulation
- **DSLs**: Blocks enable domain-specific languages (e.g., Rails migrations)

---

## Bash

```bash
for f in *.txt; do done                 # glob expansion
for f in a b c; do done                 # list iteration
for ((i=0; i<10; i++)); do done         // C-style counted loop
while read -r line; do done             # read until EOF
while [[ $n -gt 0 ]]; do done           # pre-test conditional
until [[ $n -eq 0 ]]; do done           # negated pre-test conditional
```

**Characteristics:**
- `for` does glob expansion and word splitting by default
- C-style `for ((init; cond; incr))` is a Bash extension (not POSIX)
- `while read` is the idiom for line-by-line file processing
- `until` is the negation of `while`
- Quoting and expansion rules make Bash loops tricky to get right

**Use Cases:**
- **File globbing**: `for f in *.txt` iterates matched files
- **Command-line lists**: Simple word iteration
- **Line processing**: `while read` for text manipulation
- **Conditional repetition**: `while` and `until` for polling or retries

---

## Lua

```lua
for i = 1, 10 do end                    -- numeric loop (inclusive end)
for i = 10, 1, -1 do end                -- with step (can be negative)
for i = 1, 10, 2 do end                 -- step by 2
for k, v in pairs(t) do end             -- generic table iteration
for k, v in ipairs(t) do end            -- sequence (integer-key) iteration
while cond do end                       -- pre-test conditional
repeat until cond                       -- post-test conditional

-- Table iteration alternatives
local function iter(t)
    local i = 0
    return function()
        i = i + 1
        if i <= #t then return t[i] end
    end
end
for x in iter(list) do end              -- stateless iterator
```

**Characteristics:**
- Numeric `for` has a built-in step parameter (default 1)
- `pairs()` iterates tables in unspecified order; `ipairs()` iterates sequences in order
- `repeat...until` is a post-test loop (body runs at least once)
- Lua's iterator protocol allows custom iteration via functions
- No `while let` pattern matching, but iterators via first-class functions

**Use Cases:**
- **Game scripting**: Lua's loops are lightweight and fit embedded contexts
- **Numeric sequences**: The numeric `for` is optimized in many Lua implementations
- **Table traversal**: `pairs()` for key-value iteration
- **Custom iterators**: First-class functions for stateful iteration

---

## Haskell

```haskell
-- No loop keywords. Iteration via recursion or higher-order functions.

-- Explicit recursion
sumList [] = 0
sumList (x:xs) = x + sumList xs

-- mapM_ (print each element)
mapM_ print [1..10]

-- forM_ (apply monadic action to each element)
forM_ [1..10] $ \i -> print i

-- foldr (fold right)
foldr (+) 0 [1..10]

-- foldl (fold left)
foldl (+) 0 [1..10]

-- List comprehension (generator syntax)
[x | x <- [1..10], x `mod` 2 == 0]

-- iterate (infinite sequence)
take 10 (iterate (*2) 1)  -- [1, 2, 4, 8, ...]

-- repeat (repeat forever)
take 5 (repeat 'a')  -- ['a', 'a', 'a', 'a', 'a']

-- cycle (repeat sequence)
take 5 (cycle [1,2,3])  -- [1, 2, 3, 1, 2]
```

**Characteristics:**
- No imperative loop constructs — computation is expressed as recursion or combinators
- Lazy evaluation means infinite sequences can be defined and partially consumed
- List comprehensions provide intuitive syntax reminiscent of mathematical set notation
- Folds (`foldr`, `foldl`) generalize iteration and aggregation
- Monadic loops (`mapM_`, `forM_`) handle side effects in pure functional code

**Use Cases:**
- **Functional transformation**: Folds and map-like functions for data processing
- **Lazy evaluation**: Infinite sequences and on-demand computation
- **Declarative queries**: List comprehensions for readable filtering and projection
- **Effect handling**: Monadic loops for I/O and other impure operations

---

## SQL

SQL is inherently set-based. Loops are implicit in the query engine, but procedural dialects add explicit loops:

```sql
-- Set-based (implicit iteration)
SELECT name FROM users WHERE age > 18;

-- PL/pgSQL (procedural loops)
FOR r IN SELECT * FROM table LOOP
    -- Process row r
END LOOP;

-- WHILE loop
WHILE condition LOOP
    -- Perform actions
    -- Must manually exit
END LOOP;

-- LOOP with explicit exit condition
LOOP
    -- Perform actions
    EXIT WHEN condition;
END LOOP;

-- Cursor-based iteration
DECLARE
    cur CURSOR FOR SELECT * FROM table;
    rec table%ROWTYPE;
BEGIN
    OPEN cur;
    LOOP
        FETCH cur INTO rec;
        EXIT WHEN cur%NOTFOUND;
        -- Process rec
    END LOOP;
    CLOSE cur;
END;

-- Other SQL dialects
-- T-SQL (SQL Server)
DECLARE @i INT = 0;
WHILE @i < 10 BEGIN
    PRINT @i;
    SET @i = @i + 1;
END;

-- MySQL
DECLARE done INT DEFAULT FALSE;
DECLARE cur CURSOR FOR SELECT id FROM users;
DECLARE CONTINUE HANDLER FOR NOT FOUND SET done = TRUE;
OPEN cur;
loop_label: LOOP
    FETCH cur INTO user_id;
    IF done THEN
        LEAVE loop_label;
    END IF;
    -- Process row
END LOOP;
CLOSE cur;
```

**Characteristics:**
- Primary paradigm is set-based operations (declarative)
- Procedural dialects (PL/pgSQL, T-SQL, MySQL) add imperative loops for complex logic
- Cursor-based iteration is explicit but verbose
- Row-at-a-time processing is slower than set-based operations
- Most SQL work should avoid explicit loops in favor of set operations

**Use Cases:**
- **Data queries**: Set-based queries for filtering and aggregation (preferred)
- **Complex procedures**: Loops in stored procedures when set-based logic is insufficient
- **Row processing**: Cursors for iterating when side effects are required
- **Trigger logic**: Procedural loops for complex validation or transformation

---

## TypeScript

```typescript
for (let i = 0; i < 10; i++) { }        // counted loop
for (const x of iterable) { }           // iteration protocol
for (const k in obj) { }                // key enumeration
while (cond) { }                        // pre-test conditional
do { } while (cond);                    // post-test conditional
arr.forEach(x => { });                  // array method
arr.map(x => x * 2);                    // transformation

// Async iteration
for await (const x of asyncIterable) { }

// Type-safe iteration with type guards
for (const item of array) {
    if (typeof item === 'string') {
        // item is narrowed to string
    }
}
```

**Characteristics:**
- Inherits all JavaScript loop forms
- Type safety on loop variables and iterables
- Discriminated unions enable type narrowing within loops
- Generics and utility types work with loop patterns

---

## Kotlin

```kotlin
for (i in 1..10) { }                    // range (inclusive)
for (i in 1 until 10) { }               // range (exclusive end)
for (i in 10 downTo 1) { }              // descending range
for (i in 1..10 step 2) { }             // range with step
for (x in collection) { }               // collection iteration
for ((k, v) in map) { }                 // destructuring
while (cond) { }                        // pre-test conditional
do { } while (cond);                    // post-test conditional

// Functional alternatives
collection.forEach { x -> }
collection.map { x -> x * 2 }
collection.filter { x -> x > 5 }
collection.fold(0) { acc, x -> acc + x }

// Sequence (lazy evaluation)
sequenceOf(1, 2, 3)
    .filter { it > 1 }
    .map { it * 2 }
    .forEach { println(it) }
```

**Characteristics:**
- Ranges are first-class with inclusive (`..`) and exclusive (`until`) variants
- Step function provides fine-grained control over iteration
- Destructuring in `for` loops for elegant tuple/pair unpacking
- Sequences provide lazy evaluation, similar to Rust iterators
- Inline lambdas avoid creating Function objects in many cases

---

## Swift

```swift
for i in 1...10 { }                     // closed range (inclusive)
for i in 1..<10 { }                     // half-open range (exclusive end)
for i in stride(from: 0, to: 10, by: 2) { }  // range with step
for x in collection { }                 // collection iteration
for (i, x) in collection.enumerated() { }    // enumeration
var i = 0
while i < 10 { i += 1 }                 // pre-test conditional
repeat { } while cond                   // post-test conditional

// Functional alternatives
collection.forEach { x in }
collection.map { $0 * 2 }
collection.filter { $0 > 5 }
collection.reduce(0) { $0 + $1 }

// Lazy sequences
collection.lazy.filter { $0 > 5 }.map { $0 * 2 }

// Async iteration
for try await item in asyncSequence { }
```

**Characteristics:**
- Ranges are value types with distinct semantics (closed vs. half-open)
- `stride()` provides rich control over numeric iteration
- `.enumerated()` elegantly unpacks index and value
- `repeat...while` is the post-test loop (named after Ruby's style)
- Lazy sequences defer computation until elements are consumed
- Async iteration with `try await`

---

## PHP

```php
for ($i = 0; $i < 10; $i++) { }         // counted loop
foreach ($array as $value) { }          // collection iteration
foreach ($array as $key => $value) { }  // key-value iteration
while ($cond) { }                       // pre-test conditional
do { } while ($cond);                   // post-test conditional

// Higher-order functions (functional style)
array_map(fn($x) => $x * 2, $array);
array_filter($array, fn($x) => $x > 5);
array_reduce($array, fn($acc, $x) => $acc + $x, 0);

// Generator (lazy evaluation)
function range_gen($start, $end) {
    for ($i = $start; $i <= $end; $i++) {
        yield $i;
    }
}
foreach (range_gen(1, 10) as $x) { }
```

**Characteristics:**
- `foreach` iterates arrays and objects implementing `Traversable`
- Key-value unpacking is built into `foreach`
- Higher-order functions available but not as idiomatic as in modern languages
- Generators with `yield` provide memory-efficient iteration
- Limited lazy evaluation compared to Haskell or Scala

---

## Perl

```perl
for (my $i = 0; $i < 10; $i++) { }      # counted loop (C-style)
foreach my $x (@array) { }              # array iteration
foreach my $x (@array) { ... next; }    # next (continue to next iteration)
foreach my $x (@array) { ... last; }    # last (break out of loop)
while ($cond) { }                       # pre-test conditional
do { } while ($cond);                   # post-test conditional
until ($cond) { }                       # negated pre-test conditional
do { } until ($cond);                   # negated post-test conditional

# Iterator-style (map, grep)
map { $_ * 2 } @array
grep { $_ > 5 } @array

# Foreach with range
foreach my $i (1..10) { }
foreach my $i (reverse 1..10) { }
```

**Characteristics:**
- `foreach` and `for` are synonyms (both work for iteration)
- `next` and `last` provide loop control (continue and break)
- `until` is the negation of `while`
- `map` and `grep` are higher-order functions (not keywords)
- Perl's `$_` default variable simplifies many loop constructs

---

## Clojure

```clojure
;; doseq (imperative iteration)
(doseq [x (range 10)] (println x))
(doseq [x [1 2 3] y [4 5 6]] (println x y))  ; nested

;; dotimes (repeat N times)
(dotimes [i 10] (println i))

;; map, filter, reduce (functional)
(map (fn [x] (* x 2)) [1 2 3])
(filter (fn [x] (> x 5)) [1 2 3 4 5 6])
(reduce + 0 [1 2 3 4 5])

;; Lazy sequences
(take 10 (iterate (fn [x] (* x 2)) 1))

;; loop/recur (tail recursion)
(loop [i 0]
  (if (< i 10)
    (do (println i)
        (recur (inc i)))))

;; Sequence comprehension
(for [x [1 2 3] y [4 5 6]]
  [x y])
```

**Characteristics:**
- Primarily functional: recursion and higher-order functions over imperative loops
- `doseq` and `dotimes` handle imperative iteration when side effects are needed
- Lazy sequences allow infinite generators and deferred computation
- `loop/recur` provides tail-call optimization for recursive iteration
- Sequence comprehension syntax similar to Haskell's list comprehensions

---

## F#

```fsharp
// for...to (numeric range)
for i = 1 to 10 do
    printfn "%d" i

// for...downto (descending range)
for i = 10 downto 1 do
    printfn "%d" i

// for...in (collection iteration)
for x in [1..10] do
    printfn "%d" x

// while loop
while cond do
    // actions

// Functional alternatives
List.iter (fun x -> printfn "%d" x) [1..10]
List.map (fun x -> x * 2) [1..10]
List.filter (fun x -> x > 5) [1..10]
List.fold (fun acc x -> acc + x) 0 [1..10]

// Sequence expressions (lazy)
seq { for i in 1..10 do yield i * 2 }

// Comprehension syntax
[ for i in 1..10 do i * 2 ]
```

**Characteristics:**
- Numeric ranges are first-class: `1..10`, `10..1` (descending)
- Collection iteration via `for...in`
- Strong emphasis on functional composition
- Sequence expressions provide lazy evaluation
- Pattern matching can be combined with iteration

---

## Scala

```scala
// for-comprehension (most idiomatic)
for (i <- 1 to 10) { println(i) }
for (x <- list) { println(x) }
for (x <- list; if x > 5) { println(x) }  // with filter
for (x <- list; y <- list; if x < y) println((x, y))  // nested

// for-yield (returns a collection)
val result = for (x <- list) yield x * 2

// while loop
while (cond) { /* ... */ }

// Functional alternatives
list.foreach(x => println(x))
list.map(x => x * 2)
list.filter(x => x > 5)
list.fold(0)((acc, x) => acc + x)

// Iterator (lazy)
Iterator.range(1, 11).foreach(println)

// Tail recursion
@tailrec
def loop(i: Int): Unit =
    if (i < 10) {
        println(i)
        loop(i + 1)
    }
```

**Characteristics:**
- For-comprehension syntax is syntactic sugar for `flatMap` and `map` chains
- Filters can be embedded in for-loops with `if` guards
- For-yield collects results into a new collection
- Strong functional programming support alongside imperative constructs
- Lazy iterators and tail-call optimization for efficient recursion

---

## Comparison and Design Patterns

### Loop Constructs by Category

**Counted Loops:**
- C, C++, Java, C#, JavaScript, Go, Bash, Lua, TypeScript, Kotlin, Swift, PHP, Perl: All provide explicit counted loops
- Python, Ruby, Haskell, Clojure: Favor iteration over collections; use range/sequence generators for counts

**Post-Test Loops (body runs at least once):**
- C, C++, Java, C#, JavaScript, Ruby, Lua, PHP, Perl: `do...while` or `begin...end while`
- Useful for: Input validation, prompt-response patterns, guaranteeing one execution
- NOT in: Python, Go, Rust, Haskell, Clojure

**Iterator/Collection Iteration:**
- Nearly universal: `for x in collection` (Python), `foreach` (C#), `for...in` (JavaScript), `.each` (Ruby), `for...of` (Go)
- Preferred over index-based iteration in modern languages

**Infinite Loops:**
- Explicit: Rust's `loop`, Go's `for`, Ruby's `loop`
- Implied: C, C++, JavaScript with `while (true)`
- Functional languages: `iterate` or generators

**Async Iteration:**
- JavaScript, TypeScript, Swift, C#: `for await...of`, `for try await...in`
- Essential in async-first contexts

**Functional Alternatives:**
- `map`, `filter`, `fold`/`reduce`: Nearly universal
- More declarative, often faster, and side-effect-free
- Preferred in: Rust, Haskell, Clojure, Scala, F#; increasingly popular in: JavaScript, Python, Java, Go, Ruby

### Loop-Else (Python Unique)
- Runs if loop completes without `break`
- Useful for search patterns: "loop through and break if found, otherwise not found"

### Key Insights

1. **Simplicity vs. Flexibility**: Go intentionally uses one `for` keyword; Rust intentionally omits `do-while`
2. **Functional is Rising**: Even traditionally imperative languages now emphasize `map`, `filter`, `reduce`
3. **Lazy Evaluation**: Haskell, Clojure, Rust, and newer Python/JavaScript features show the value of deferred computation
4. **Type Safety**: TypeScript, Kotlin, Swift, Scala combine loop iteration with type safety and pattern matching
5. **Async-First**: Modern languages (Rust, Kotlin, Swift) provide native async iteration constructs
