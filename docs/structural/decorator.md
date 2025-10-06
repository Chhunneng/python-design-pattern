# Decorator (Structural)

Reference: `https://python-patterns.guide/gang-of-four/principles/#solution-3-the-decorator-pattern`

Note: This page covers the object‑oriented Decorator pattern (wrapping objects). Python also has function decorators (`@decorator`), which are related but not identical.

## What is it?

Attach new responsibilities to objects by wrapping them in a series of lightweight wrappers at runtime.

## Why use it?

- Add features without modifying or subclassing the original class.
- Combine multiple behaviors flexibly (e.g., caching + logging).

## How to use it

- Define a component interface.
- Implement the base component.
- Implement decorators that implement the same interface and hold a wrapped component.

## When to use

- Features are optional or combinable per object instance.
- You want to avoid a wide subclass tree for every combination.

## Best practices

- Keep wrappers transparent: they should forward calls to the wrapped object.
- Order of decorators can matter — document expected stacking.

## Example

```python
from __future__ import annotations
from abc import ABC, abstractmethod


class Text(ABC):
    @abstractmethod
    def render(self) -> str: ...


class PlainText(Text):
    def __init__(self, content: str) -> None:
        self.content = content

    def render(self) -> str:
        return self.content


class TextDecorator(Text):
    def __init__(self, inner: Text) -> None:
        self.inner = inner

    def render(self) -> str:  # default: transparent
        return self.inner.render()


class Bold(TextDecorator):
    def render(self) -> str:
        return f"<b>{self.inner.render()}</b>"


class Italic(TextDecorator):
    def render(self) -> str:
        return f"<i>{self.inner.render()}</i>"


text = Italic(Bold(PlainText("hello")))
print(text.render())  # <i><b>hello</b></i>
```
