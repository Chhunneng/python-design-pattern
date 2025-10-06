# Prototype

Reference: `https://python-patterns.guide/gang-of-four/creational/#the-prototype-pattern`

## What is it?

Create new objects by copying a prototype instead of instantiating classes. In Python, use `copy.copy` / `copy.deepcopy` or custom `.clone()`.

## Why use it?

- Object creation is expensive or complex to configure.
- You need many similar objects with small differences.

## How to use it

- Keep a prepared prototype; clone it and tweak the differences.

## When to use

- Heavy setup cost; cloning is cheaper.

## Best practices

- Be explicit about shallow vs deep copy.
- For mutable graphs, implement a clear `.clone()` contract.

## Example

```python
import copy


class Document:
    def __init__(self, title: str, sections: list[str]) -> None:
        self.title = title
        self.sections = sections

    def clone(self) -> "Document":
        # deep copy mutable parts
        return Document(self.title, copy.deepcopy(self.sections))


base = Document("Report", ["Intro", "Methods", "Results"]) 
v1 = base.clone()
v1.sections.append("Discussion")

print(base.sections)  # unchanged
```
