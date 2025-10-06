# Builder

Reference: `https://python-patterns.guide/gang-of-four/creational/#the-builder-pattern`

## What is it?

Separate construction of a complex object from its representation to build it step by step.

## Why use it?

- Object has many optional parts or validation rules.
- You want readable chained configuration.

## How to use it

- Provide methods to set parts and a final `build()` to validate and return the object.

## When to use

- Alternatives like many constructor parameters are hard to read/validate.

## Best practices

- Validate in `build()`; keep intermediate state private.
- Prefer immutable result objects.

## Example

```python
from dataclasses import dataclass


@dataclass(frozen=True)
class User:
    name: str
    email: str
    age: int | None = None


class UserBuilder:
    def __init__(self) -> None:
        self._name: str | None = None
        self._email: str | None = None
        self._age: int | None = None

    def name(self, value: str) -> "UserBuilder":
        self._name = value
        return self

    def email(self, value: str) -> "UserBuilder":
        self._email = value
        return self

    def age(self, value: int) -> "UserBuilder":
        self._age = value
        return self

    def build(self) -> User:
        if not self._name or not self._email:
            raise ValueError("name and email are required")
        return User(self._name, self._email, self._age)


user = UserBuilder().name("Ada").email("ada@example.com").age(36).build()
```
