# Abstract Factory

Reference: `https://python-patterns.guide/gang-of-four/creational/#the-abstract-factory-pattern`

## What is it?

Provide an interface for creating related objects without specifying their concrete classes. In Python, callable factories are often enough.

## Why use it?

- Build families of related objects that must work together.
- Swap entire families (themes, drivers) at once.

## How to use it

- Define factory callables or a factory object with methods to create each product.
- Pass the factory into code that needs to create products.

## When to use

- You switch between environments/skins/databases that must remain consistent.

## Best practices

- Prefer simple callables over complex factory classes in Python.
- Keep product interfaces stable; let factories vary.

## Example

```python
class Button:
    def __init__(self, label: str) -> None:
        self.label = label


class DarkButton(Button):
    ...


class LightButton(Button):
    ...


class UIAbstractFactory:
    def __init__(self, button_factory):
        self.button_factory = button_factory

    def create_button(self, label: str) -> Button:
        return self.button_factory(label)


dark_ui = UIAbstractFactory(DarkButton)
light_ui = UIAbstractFactory(LightButton)

ok_dark = dark_ui.create_button("OK")
ok_light = light_ui.create_button("OK")
```
