# Factory Method

Reference: `https://python-patterns.guide/gang-of-four/creational/#the-factory-method-pattern`

## What is it?

Define a method responsible for creating objects, allowing subclasses or configurations to decide which class to instantiate.

## Why use it?

- Centralize object creation logic.
- Defer the choice of concrete class.

## How to use it (Pythonic)

- Prefer passing a callable (class or function) as a dependency. Your code calls it to create objects.

## When to use

- You need to create objects that vary by environment/configuration.

## Best practices

- Accept any callable returning the desired interface.
- Keep the creation method small and obvious.

## Example

```python
class Storage:
    def save(self, data: str) -> None: ...


class FileStorage(Storage):
    def __init__(self, path: str) -> None:
        self.path = path

    def save(self, data: str) -> None:
        with open(self.path, "a", encoding="utf-8") as f:
            f.write(data + "\n")


def create_storage_factory(path: str):
    # Factory method returns a callable that builds storages
    def factory() -> Storage:
        return FileStorage(path)
    return factory


storage_factory = create_storage_factory("data.log")
storage = storage_factory()
storage.save("hello")
```
