# Null Object (Python‑specific)

Reference: `https://python-patterns.guide/python-specific/`

## What is it?

Provide an object that implements the expected interface but does nothing, avoiding `None` checks.

## Why use it?

- Simplify code by removing repeated conditionals.

## How to use it

- Implement a do‑nothing class with the same methods and use it as a default.

## When to use

- Optional collaborators where “do nothing” is acceptable.

## Best practices

- Ensure behavior is safe and explicit; document that it’s a no‑op.

## Example

```python
class Logger:
    def log(self, msg: str) -> None:
        print(msg)


class NullLogger(Logger):
    def log(self, msg: str) -> None:
        pass


def process(data: str, logger: Logger | None = None) -> None:
    logger = logger or NullLogger()
    logger.log("start")
    # ... work ...
    logger.log("done")
```
