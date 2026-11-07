# Euclid's algorithm: the greatest common divisor

The greatest common divisor (GCD) of two whole numbers is the largest number
that divides both. For 48 and 18 it is 6. You could find it by trying every
number from the smaller value down to 1, but Euclid found a much shorter way
more than two thousand years ago.

> [!NOTE]
> Boxes like this one explain how `unravel` turns this article into a Python
> program. You can skip them if you only want to learn the algorithm.

## The key observation

Any number that divides both `a` and `b` also divides their difference, and
therefore also divides `a % b`, the remainder after dividing `a` by `b`. So the
pair `(a, b)` has the same common divisors as the pair `(b, a % b)`, and the
second pair is smaller.

Repeat the step until the remainder is 0. For 48 and 18:

| a | b | a % b |
| --- | --- | --- |
| 48 | 18 | 12 |
| 18 | 12 | 6 |
| 12 | 6 | 0 |

When the remainder becomes 0, `b` divides `a` exactly, so `b` is the answer: 6.

## Write the function

The function repeats the step until `b` is 0, then returns `a`.

> [!NOTE]
> A code block whose opening line has a name in double angle brackets, such as
> `<<the gcd function>>`, defines a **chunk**. Inside a chunk, a line such as
> `<<replace the pair with a smaller one>>` is a **reference**: `unravel`
> replaces it with that chunk's code, keeping its indentation. A chunk can be
> referenced before it is defined.

*`<<the gcd function>>`* `=`

```python <<the gcd function>>
def gcd(a: int, b: int) -> int:
    """Return the greatest common divisor of two non-negative integers."""
    while b != 0:
        <<replace the pair with a smaller one>>
    return a
```

Each step replaces `(a, b)` with `(b, a % b)`. Python's tuple assignment
computes both new values before assigning either one.

*`<<replace the pair with a smaller one>>`* `=`

```python <<replace the pair with a smaller one>>
a, b = b, a % b
```

## Check the result

The worked example gives the first check. A second check covers a pair with no
common divisor except 1, and a third covers a zero.

*`<<gcd checks>>`* `=`

```python <<gcd checks>>
assert gcd(48, 18) == 6
assert gcd(17, 5) == 1
```

The order of the arguments should not matter. If `a` is smaller than `b`, the
first step swaps them, because `a % b` is just `a`.

> [!NOTE]
> Defining a chunk with a name that is already in use adds to that chunk.
> `unravel` joins the definitions in document order.

*`<<gcd checks>>`* `=` (continued)

```python <<gcd checks>>
assert gcd(18, 48) == 6
assert gcd(7, 0) == 7
```

## Assemble the program

> [!NOTE]
> A chunk whose name looks like a filename, such as `gcd.py`, is an **output
> file**. `unravel` writes it with every reference expanded, so code can be
> explained in whatever order suits the reader and still come out in the order
> Python needs.

*`<<gcd.py>>`* `=`

```python <<gcd.py>>
<<the gcd function>>


if __name__ == "__main__":
    <<gcd checks>>
    print("All checks passed.")
```

> [!NOTE]
> Generate and run the program from the repository root:
>
> ```bash
> uv run unravel example.md -o generated
> uv run python generated/gcd.py
> ```
