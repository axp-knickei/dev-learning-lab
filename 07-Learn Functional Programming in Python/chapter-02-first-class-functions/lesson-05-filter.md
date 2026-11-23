# Python filter Function

## Lecture information

- **Course name:** Learn Functional Programming in Python
- **Chapter:** 2 — First-Class Functions
- **Lesson:** 5 (L5)
- **Lecture title:** Filter
- **Programming language:** Python 3
- **Original lecture URL:** Filter on Boot.dev — URL not yet supplied.

## 1. Lecture overview

This lecture introduces Python's built-in `filter` function, a core tool in functional programming. In functional paradigms, manipulating collections through declarative higher-order functions is preferred over imperative loops that manually manage iteration state and mutable accumulator variables.

The `filter` function extracts elements from an iterable that satisfy a specified predicate function (a function returning `True` or `False`). Understanding `filter` is essential for mastering higher-order functions—functions that accept other functions as arguments—and establishes a foundation for data transformation pipelines alongside functions like `map` and `reduce`.

## 2. Learning objectives

By the end of this lecture, you should be able to:

- Define what a higher-order function and a predicate function are.
- Understand the syntax and mechanics of Python's built-in `filter` function.
- Combine `filter` with lambda expressions for concise filtering logic.
- Convert filter iterator objects into concrete collection types such as lists or joined strings.
- Apply string splitting, line filtering, and joining to process structured text documents cleanly.

## 3. Key concepts explained

### Predicate function

**Definition:** A function that takes one or more inputs and evaluates to a boolean value (`True` or `False`).

**Explanation:** Think of it as a quality inspector at a factory checkpoint. Each item passes through; if the inspector stamps it `True`, it proceeds down the line. If `False`, it is discarded.

**Why it matters:** `filter` relies entirely on the truthiness of a predicate's result to decide whether each item is kept or dropped.

**When used:** Whenever conditional selection logic needs to be applied systematically across a collection.

### Higher-order function

**Definition:** A function that takes one or more functions as arguments, returns a function, or both.

**Explanation:** In Python, functions are "first-class citizens," meaning they can be passed around just like numbers, strings, or lists. A higher-order function uses another function as part of its algorithm.

**Why it matters:** It allows developers to abstract away the mechanics of iteration, focusing solely on the rule applied to each element.

**When used:** In data processing pipelines, event handling, and functional transformations (`map`, `filter`, `sorted`).

### The filter function

**Definition:** A built-in function with the signature `filter(function, iterable)`.

**Explanation:** As its iterator is consumed, it applies the provided function to each element of the iterable. Elements for which the function returns a truthy value are retained.

**Why it matters:** It avoids manual `for` loops and temporary storage lists, providing a clear and declarative syntax.

**When used:** Filtering data records, cleansing text streams, selecting subsets of numerical observations.

## 4. Code examples and explanations

### Example 1: Basic list filtering with a named function

```python
def is_positive(num: int) -> bool:
    return num > 0


measurements: list[int] = [-3, 12, 0, -8, 25, 4]
positive_measurements: list[int] = list(filter(is_positive, measurements))

print(positive_measurements)
```

**Line-by-line explanation**

- `def is_positive(num: int) -> bool:` defines a predicate function expecting an integer and returning a boolean.
- `return num > 0` returns `True` if `num` is strictly greater than zero, and `False` otherwise.
- `measurements: list[int] = [...]` defines an input list with mixed integers.
- `filter(is_positive, measurements)` creates an iterator yielding only items where `is_positive(item)` evaluates to `True`.
- `list(...)` consumes the iterator and collects the retained elements into a new list.
- `print(...)` displays the resulting list.

**Output**

```text
[12, 25, 4]
```

**Why it behaves this way**

`filter` evaluates `-3 > 0` (`False`), `12 > 0` (`True`), `0 > 0` (`False`), and so forth. Only `12`, `25`, and `4` pass the condition.

### Example 2: Text processing using filter, lambda, and string methods

```python
raw_log: str = "INFO: Boot sequence started\nDEBUG: Initializing memory\nERROR: Disk full\nDEBUG: Verification complete"

# Retain only non-debug lines
clean_lines = filter(
    lambda line: not line.startswith("DEBUG:"), raw_log.split("\n")
)
sanitized_log: str = "\n".join(clean_lines)

print(sanitized_log)
```

**Line-by-line explanation**

- `raw_log: str = ...` defines a multiline string containing mock log data.
- `raw_log.split("\n")` breaks the single string into a list of individual lines: `['INFO: ...', 'DEBUG: ...', 'ERROR: ...', 'DEBUG: ...']`.
- `lambda line: not line.startswith("DEBUG:")` creates an anonymous function returning `True` for any line that does not begin with `"DEBUG:"`.
- `filter(...)` applies this condition across all lines as the iterator is consumed.
- `"\n".join(...)` consumes the iterator produced by `filter` and joins the remaining lines with newline characters.

**Output**

```text
INFO: Boot sequence started
ERROR: Disk full
```

## 5. Common mistakes and misconceptions

### 1. Passing a function call instead of the function reference

Incorrect:

```python
filter(is_positive(), measurements)  # Raises TypeError
```

Correct:

```python
filter(is_positive, measurements)
```

**Explanation:** `filter` needs a reference to the function object so that it can call it on each item internally. Calling `is_positive()` immediately executes it once with missing arguments.

### 2. Expecting filter to return a list directly

Incorrect assumption:

```python
result = filter(lambda x: x % 2 == 0, [1, 2, 3, 4])
print(result[0])  # Raises TypeError: 'filter' object is not subscriptable
```

Correct:

```python
result = list(filter(lambda x: x % 2 == 0, [1, 2, 3, 4]))
print(result[0])  # Output: 2
```

**Explanation:** In Python 3, `filter` returns an iterator (a lazy generator-like object) rather than a list. It must be converted using `list()`, iterated with a loop, or consumed by a function like `"".join()`.

### 3. Inverting filter logic

Remember that `filter` keeps elements where the predicate returns a truthy value. If the objective is to remove invalid elements, the predicate must return `False` for invalid items and `True` for valid items (e.g., using `not condition`).

## 6. Practical applications

### Application 1: Bioinformatics sequence data cleaning

When parsing raw FASTA or sequence records, comment or header lines (frequently starting with `;` or `#`) must be filtered out before sequence assembly:

```python
raw_fasta_lines = [
    "; Sample sequencing run 402",
    "ATGCGATCGATCG",
    "# Low quality read below",
    "GCTAGCTAGCTA",
]

valid_sequences = list(
    filter(
        lambda seq: not (seq.startswith(";") or seq.startswith("#")),
        raw_fasta_lines,
    )
)
# Result: ['ATGCGATCGATCG', 'GCTAGCTAGCTA']
```

### Application 2: Data sanitization in automated pipelines

Removing empty records or malformed rows from a batch export:

```python
records = ["sample_1,0.45", "", "sample_2,0.89", "   ", "sample_3,0.12"]
non_empty_records = list(filter(lambda r: r.strip() != "", records))
# Result: ['sample_1,0.45', 'sample_2,0.89', 'sample_3,0.12']
```

## 7. Important takeaways

- `filter(func, iterable)` applies `func` to every item in `iterable` as it is consumed and retains only items where `func(item)` is truthy.
- In Python 3, `filter` is lazy; it returns an iterator that computes items on demand rather than allocating a full list in memory immediately.
- The predicate function argument should be passed as an uncalled callable reference or an inline lambda.
- To reconstruct a single string after line filtering, use `str.split("\n")`, filter the resulting iterable, and assemble it back with `"\n".join(...)`.
- When filtering out unwanted elements, use negation (`not`) inside the predicate so that acceptable elements evaluate to `True`.

## 8. Self-assessment questions

1. What data type is returned by the expression `filter(lambda x: True, [1, 2, 3])` in Python 3?
2. If the predicate passed to `filter` returns `None` for every item, what will `list(filter(...))` yield?
3. Why does `filter(str.isalpha, ["a", "1", "b"])` work without writing an explicit lambda?
4. How does `filter` differ from list comprehensions such as `[x for x in data if condition(x)]`?

<details>
<summary>Click to view answers</summary>

1. A filter iterator object (not a list).
2. An empty list (`[]`), because `None` is falsy in Python.
3. `str.isalpha` is already a callable function that accepts a string argument and returns a boolean, satisfying the requirements of a predicate function.
4. Functionally they achieve similar results, but `filter` is a higher-order function that works directly with iterators lazily, while a list comprehension evaluates immediately into an in-memory list (unless written as a generator expression `(x for x in data if condition(x))`).

</details>

## 9. Further learning

### Connected curriculum concepts

- `map()`: Transform every element in an iterable rather than selectively excluding items.
- `functools.reduce()`: Accumulate elements of an iterable into a single cumulative result.
- Lambda expressions: Anonymous inline functions commonly used with higher-order functions.

### Supplementary exploration

- Generator expressions as an idiomatic Python alternative to `filter`.
- Passing `None` as the first argument to `filter(None, iterable)` to quickly strip falsy values (like `0`, `""`, and `None`) from collections.

## 10. Personal learning reflections

- [Additional note: Understanding `int` and `bool` type hints](lesson-05-filter-type-hints.md) — 2026-11-23 09:53:12 (Asia/Bangkok, UTC+07:00).

- **What I understood:**
- **What I found difficult:**
- **Mistakes I made:**
- **How I solved them:**
- **What I need to review:**
- **Date completed:**
