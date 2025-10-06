# Constant (Python‑specific)

Reference: `https://python-patterns.guide/python-specific/`

## What is it?

Expose read‑only values at module level to signal configuration that won’t change at runtime.

## Why use it?

- Clear, simple, importable values used across modules.

## How to use it

- Define uppercase names in a module and import them elsewhere.

## When to use

- Values known at import time: versions, limits, feature flags.

## Best practices

- Use UPPER_SNAKE_CASE. Avoid doing I/O at import time.

## Example

```python
# constants.py
API_BASE_URL = "https://api.example.com"
MAX_RETRIES = 3

# usage
from constants import API_BASE_URL, MAX_RETRIES
```
