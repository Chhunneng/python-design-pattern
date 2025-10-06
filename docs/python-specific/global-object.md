# Global Object (Python‑specific)

Reference: `https://python-patterns.guide/python-specific/`

## What is it?

Provide a single shared object (often at module level) used across the program.

## Why use it?

- Simple way to share state or services.

## How to use it

- Create the object at module import; other modules import and use it.

## When to use

- Truly shared, app‑wide service/config. Beware of tight coupling and testing friction.

## Best practices

- Prefer passing dependencies explicitly. If using globals, make them simple and immutable when possible.

## Example

```python
# logger.py
class Logger:
    def log(self, msg: str) -> None:
        print(msg)


LOGGER = Logger()  # global object


# usage
from logger import LOGGER
LOGGER.log("started")
```
