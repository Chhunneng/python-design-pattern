# Sentinel Object (Python‑specific)

Reference: `https://python-patterns.guide/python-specific/`

## What is it?

A unique object used to signal a special meaning like “no value provided,” distinct from `None` or other valid values.

## Why use it?

- Distinguish “argument not passed” from “passed None.”

## How to use it

- Create a module‑level unique object: `MISSING = object()` and compare by identity.

## When to use

- Function defaults where `None` is a valid value.

## Best practices

- Keep it private to the module if not part of the API.

## Example

```python
MISSING = object()


def get(key, default=MISSING):
    if default is MISSING:
        # behave differently when no default provided
        raise KeyError(key)
    return default
```
