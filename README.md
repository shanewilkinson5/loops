# Loops

A list of loop constructs across programming languages.

Each entry shows the loop forms the language actually provides — counted loops,
conditional loops, iteration over collections, and whatever the language calls
its post-test loop (if it has one).

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

---

## C

```c
for (int i = 0; i < 10; i++) { }        /* counted */
while (cond) { }                         /* pre-test */
do { } while (cond);                     /* post-test */
goto top;                                /* unstructured */
```

No built-in collection iteration — you index or walk a pointer.

## C++

```cpp
for (int i = 0; i < 10; ++i) { }         // counted
for (auto& x : container) { }            // range-based (C++11)
while (cond) { }
do { } while (cond);
std::for_each(v.begin(), v.end(), fn);   // algorithm, not a keyword
```

## Java

```java
for (int i = 0; i < 10; i++) { }         // counted
for (String s : list) { }                // enhanced for
while (cond) { }
do { } while (cond);
list.forEach(s -> { });                  // Iterable.forEach (Java 8)
```

## C#

```csharp
for (int i = 0; i < 10; i++) { }
foreach (var x in collection) { }
while (cond) { }
do { } while (cond);
```

`foreach` works on anything implementing `IEnumerable`.

## Python

```python
for x in iterable: ...        # the only counted-ish loop; use range() for counts
while cond: ...
for i in range(10): ...

# both loop forms accept else, which runs if no break fired
for x in xs:
    ...
else:
    ...
```

No do-while. No C-style three-clause `for`.

## JavaScript

```javascript
for (let i = 0; i < 10; i++) { }   // counted
for (const x of iterable) { }      // values
for (const k in obj) { }           // enumerable keys
while (cond) { }
do { } while (cond);
arr.forEach(x => { });             // method, cannot break
for await (const x of asyncIt) { } // async iteration
```

## Go

```go
for i := 0; i < 10; i++ { }   // counted
for cond { }                   // while-style
for { }                        // infinite
for i, v := range slice { }    // range
for i := range 10 { }          // range-over-int (Go 1.22)
```

One keyword, five forms. No do-while.

## Rust

```rust
for x in iterable { }          // consumes an IntoIterator
while cond { }
while let Some(x) = it.next() { }
loop { }                       // infinite; can break with a value
```

`loop` is an expression: `let x = loop { break 5; };`

## Ruby

```ruby
5.times { |i| }
(1..10).each { |i| }
collection.each { |x| }
while cond; end
until cond; end
loop { break if done }
begin; end while cond          # post-test
```

## Bash

```bash
for f in *.txt; do done
for ((i=0; i<10; i++)); do done
while read -r line; do done
until cond; do done
```

## Lua

```lua
for i = 1, 10 do end            -- numeric
for i = 10, 1, -1 do end        -- with step
for k, v in pairs(t) do end     -- generic
while cond do end
repeat until cond               -- post-test
```

## Haskell

No loop keywords. Iteration is recursion or a combinator.

```haskell
mapM_ print [1..10]
forM_ [1..10] $ \i -> print i
foldr (+) 0 [1..10]
```

## SQL

Set-based by default — the loop is implicit. Procedural dialects add real ones:

```sql
-- PL/pgSQL
FOR r IN SELECT * FROM t LOOP END LOOP;
WHILE cond LOOP END LOOP;
LOOP EXIT WHEN cond; END LOOP;
```

---

## Notes

- **Post-test loops** (body runs at least once) exist in C, C++, Java, C#,
  JavaScript, Ruby, Lua, and Pascal — but not Python, Go, or Rust.
- **`for`-`else`** is essentially unique to Python.
- **Loop-as-expression** (returns a value) shows up in Rust's `loop`.
