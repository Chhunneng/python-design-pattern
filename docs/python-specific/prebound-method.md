# Prebound Method (Python‑specific)

Reference: `https://python-patterns.guide/python-specific/`

## What is it?

Store a bound method ahead of time to avoid repeatedly looking it up on the instance.

## Why use it?

- Micro‑optimization and convenience when calling the same method many times.

## How to use it

- Assign `local = obj.method` and call `local()` in a tight loop.

## When to use

- Performance hotspots; otherwise, keep code simple.

## Best practices

- Only when profiling shows attribute lookup overhead matters.

## Example

```python
append = [].append  # prebound
for i in range(3):
    append(i)
```
