# Iterator

Reference: `https://python-patterns.guide/gang-of-four/behavioral/#the-iterator-pattern`

## What is it?

Provide a way to access elements of a collection sequentially without exposing its internal representation. Python builds this into the iteration protocol.

## Why use it?

- Clean separation between data structure and traversal.
- Python `for` loop and comprehensions support any iterable.

## How to use it (Python)

- Make an object iterable by defining `__iter__(self)` that returns an iterator.
- An iterator implements `__iter__(self)` (returns self) and `__next__(self)`.

## When to use

- You want custom traversal over your own container objects.

## Best practices

- Prefer generators (`yield`) for simple iterators.
- Document whether iteration is single‑pass or restartable.

## Example (generator)

```python
def countdown(n: int):
    while n > 0:
        yield n
        n -= 1


for i in countdown(3):
    print(i)
```

## Example (custom iterable + iterator)

```python
class Range:
    def __init__(self, start: int, end: int) -> None:
        self.start = start
        self.end = end

    def __iter__(self):
        return RangeIterator(self.start, self.end)


class RangeIterator:
    def __init__(self, start: int, end: int) -> None:
        self.current = start
        self.end = end

    def __iter__(self):
        return self

    def __next__(self):
        if self.current >= self.end:
            raise StopIteration
        value = self.current
        self.current += 1
        return value


for x in Range(0, 3):
    print(x)
```
