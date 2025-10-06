# Singleton

Reference: `https://python-patterns.guide/gang-of-four/creational/#the-singleton-pattern`

## What is it?

Ensure a class has only one instance and provide a global access point.

## Why use it?

- Coordinate shared resources (e.g., config, connection pool).

## How to use it (Pythonic)

- Prefer module‑level singletons or simple global objects over clever metaclasses.

## When to use

- You truly need exactly one instance. Beware testability and hidden coupling.

## Best practices

- Keep it simple: a module with functions/objects is often enough.
- If you need a class, expose a single instance explicitly.

## Example (module‑level singleton)

```python
# config.py
SETTINGS = {"env": "prod", "db_url": "sqlite:///app.db"}


# usage
from config import SETTINGS
print(SETTINGS["env"])  # single shared object
```

## Example (class with explicit instance)

```python
class Config:
    def __init__(self) -> None:
        self.env = "prod"


CONFIG = Config()  # explicit singleton instance
```
