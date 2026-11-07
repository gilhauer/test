# Euclid's algorithm: the greatest common divisor

<kbd>Demo</kbd> This document compares ways to separate the article (teaching
the algorithm) from the notes that explain `unravel`. Each `unravel` note uses
a different technique, labelled with a <kbd>Technique n</kbd> badge just
before it. The badges belong to the demo, not to the article.

| Technique | Markdown used | Where it appears |
| --- | --- | --- |
| 1. Quote with a lead-in | `> **About unravel:** …` | Adding to a chunk, under "Check the result" |
| 2. GitHub alert | `> [!NOTE]` | Defining and referencing chunks, under "Write the function" |
| 3. Collapsible block | `<details>` / `<summary>` | Output files, under "Assemble the program" |
| 4. Footnote | `[^name]` | Forward references, under "Write the function" |
| 5. Reading guide plus pointers | An ordinary section, then one-line pointers | "Reading this document", and a pointer under "Generate and run" |

---

The greatest common divisor (GCD) of two whole numbers is the largest number
that divides both. For 48 and 18 it is 6. You could find it by trying every
number from the smaller value down to 1, but Euclid found a much shorter way
more than two thousand years ago.

<kbd>Technique 5: reading guide</kbd>

## Reading this document

This article is also a program. Its named code blocks, called **chunks**, are
extracted by `unravel` and assembled into `gcd.py`. A line in double angle
brackets, such as `<<the gcd function>>`, stands for the chunk of that name.
The rest of the article explains the algorithm; notes about `unravel` are kept
short and point back here.

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

<kbd>Technique 2: GitHub alert</kbd>

> [!NOTE]
> A code block whose opening line has a name in double angle brackets, such as
> `<<the gcd function>>`, defines a **chunk**. Inside a chunk, a line such as
> `<<replace the pair with a smaller one>>` is a **reference**: `unravel`
> replaces it with that chunk's code, keeping its indentation.

*`<<the gcd function>>`* `=`

```python <<the gcd function>>
def gcd(a: int, b: int) -> int:
    """Return the greatest common divisor of two non-negative integers."""
    while b != 0:
        <<replace the pair with a smaller one>>
    return a
```

<kbd>Technique 4: footnote</kbd>

Each step replaces `(a, b)` with `(b, a % b)`.[^forward] Python's tuple
assignment computes both new values before assigning either one.

*`<<replace the pair with a smaller one>>`* `=`

```python <<replace the pair with a smaller one>>
a, b = b, a % b
```

## Check the result

The worked example gives the first check. A second check covers a pair with no
common divisor except 1.

*`<<gcd checks>>`* `=`

```python <<gcd checks>>
assert gcd(48, 18) == 6
assert gcd(17, 5) == 1
```

The order of the arguments should not matter. If `a` is smaller than `b`, the
first step swaps them, because `a % b` is just `a`. A zero is the other edge.

<kbd>Technique 1: quote with a lead-in</kbd>

> **About unravel:** Defining a chunk with a name that is already in use adds
> to that chunk. `unravel` joins the definitions in document order.

*`<<gcd checks>>`* `=` (continued)

```python <<gcd checks>>
assert gcd(18, 48) == 6
assert gcd(7, 0) == 7
```

## Assemble the program

The program defines the function, then runs the checks when executed as a
script.

<kbd>Technique 3: collapsible block</kbd>

<details>
<summary>How <code>unravel</code> uses this chunk</summary>

A chunk whose name looks like a filename, such as `gcd.py`, is an **output
file**. `unravel` writes it with every reference expanded, so the code can be
explained in the order that suits the reader and still come out in the order
Python needs.

</details>

*`<<gcd.py>>`* `=`

```python <<gcd.py>>
<<the gcd function>>


if __name__ == "__main__":
    <<gcd checks>>
    print("All checks passed.")
```

## Generate and run

<kbd>Technique 5: pointer</kbd>

Generate the program from the repository root (see
[Reading this document](#reading-this-document)), then run it:

```bash
uv run unravel example.md -o generated
uv run python generated/gcd.py
```

The program prints `All checks passed.`

[^forward]: This chunk is defined after the function that refers to it.
    `unravel` allows a reference to appear before the chunk's definition.
