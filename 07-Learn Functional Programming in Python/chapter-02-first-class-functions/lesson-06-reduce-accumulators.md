# Dev Log: Understanding functools.reduce Accumulators

- **Date:** November 11, 2026
- **Time:** 14:35 (timezone not specified)
- **Topic:** Functional Programming in Python — `functools.reduce`
- **Course:** Learn Functional Programming in Python
- **Chapter:** 2 — First-Class Functions
- **Lesson:** 6 — Reduce
- **Note type:** Personal learning note. The main Reduce lesson note will be added separately.

## The confusion

I got stuck looking at this helper function from the lesson:

```python
def add(sum_so_far: int, x: int) -> int:
    return sum_so_far + x
```

I had two main points of confusion:

1. I thought `sum_so_far` was a separate function or a special reserved keyword, rather than just an ordinary parameter name.
2. I did not see why addition had to take only two arguments when functions in Python can technically accept three, four, or more arguments.

## The breakthrough and explanation

### Parameter naming is arbitrary

`sum_so_far` is just a descriptive label chosen by the developer. It behaves identically to:

```python
def add(a, b):
    return a + b
```

It was named `sum_so_far` purely for human readability to signal what value `reduce` passes into that spot.

### Why reduce passes two arguments

While custom Python functions can take any number of arguments, `functools.reduce` works by chaining pairwise operations. It processes an iterable by combining:

1. **Accumulator:** The running result accumulated from all prior steps.
2. **Current item:** The next single value from the iterable.

For example, reducing `[1, 2, 3]` without an initial value calls `add(1, 2)` to produce `3`, then `add(3, 3)` to produce `6`.

Because `reduce` supplies two positional arguments at each step, its callback must accept those two arguments. A function with two parameters is the usual pattern; extra optional parameters or `*args` can also work, but extra required arguments would be missing.

Without an initial value, the first iterable item starts the accumulator. If an initial value is supplied, that value starts the accumulator instead.

## Takeaway

Whenever I write a reducer callback, I can think of it as:

```python
def reducer_step(accumulator, current_item):
    return new_accumulator
```

This is a conceptual template: `new_accumulator` stands for the result calculated in the function body.

The first slot is always the carry-over from previous iterations (or the starting accumulator), and the second slot is the new item being folded in.
