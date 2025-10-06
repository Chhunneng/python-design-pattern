# Composite

Reference: `https://python-patterns.guide/gang-of-four/structural/#the-composite-pattern`

## What is it?

Treat individual objects and groups of objects uniformly using a tree structure.

## Why use it?

- Work with a part or a whole using the same interface.
- Natural fit for hierarchies like filesystems, GUI widgets, org charts.

## How to use it

- Define a common interface for leaves and composites.
- Composites hold children and delegate operations to them.

## When to use

- Your domain is hierarchical, and you want uniform operations (e.g., `render()`, `size()`, `price()`).

## Best practices

- Keep child management (add/remove) on the composite only.
- Prefer iteration over recursion when depth can be large.

## Example

```python
from __future__ import annotations
from abc import ABC, abstractmethod
from typing import List


class Node(ABC):
    @abstractmethod
    def size(self) -> int: ...


class File(Node):
    def __init__(self, name: str, size_bytes: int) -> None:
        self.name = name
        self.size_bytes = size_bytes

    def size(self) -> int:
        return self.size_bytes


class Directory(Node):
    def __init__(self, name: str) -> None:
        self.name = name
        self.children: List[Node] = []

    def add(self, node: Node) -> None:
        self.children.append(node)

    def size(self) -> int:
        return sum(child.size() for child in self.children)


root = Directory("/")
root.add(File("a.txt", 120))
src = Directory("src")
src.add(File("main.py", 800))
root.add(src)
print(root.size())  # 920
```
