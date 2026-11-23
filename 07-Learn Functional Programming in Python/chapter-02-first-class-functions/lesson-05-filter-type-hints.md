# Filter: Understanding `int` and `bool` Type Hints

- **Date and time:** 2026-11-23 09:53:12 (Asia/Bangkok, UTC+07:00)
- **Related lesson:** [L5: Filter](lesson-05-filter.md)

## What confused me at first

```python
def is_even(x: int) -> bool:
    return x % 2 == 0


numbers: list[int] = [1, 2, 3, 4, 5, 6]
evens: list[int] = list(filter(is_even, numbers))
print(evens)
# [2, 4, 6]
```

I wasn't sure what `bool` meant in `def is_even(x: int) -> bool:`.

I wondered why `int` in `(x: int)` seemed to "change" into `bool` in `-> bool`.

## What I learned

- `bool` stands for boolean, a data type that can only be `True` or `False`.
- `(x: int)` describes the input type: a whole number going into the function.
- `-> bool` is a type hint describing the output type: the `True` or `False` value the function will return. It appears before the function's steps are written.

The expression `x % 2 == 0` produces the boolean result. The type hints describe the input and output; they do not convert `x` into a boolean or enforce these types at runtime.
