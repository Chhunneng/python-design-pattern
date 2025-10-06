# Flyweight

Reference: `https://python-patterns.guide/gang-of-four/structural/#the-flyweight-pattern`

## What is it?

Share heavy, immutable data across many objects to reduce memory usage. Externalize the changing state.

## Why use it?

- You create thousands/millions of similar objects.
- Large memory footprint comes from duplicated immutable state.

## How to use it

- Separate intrinsic (shared) from extrinsic (per‑object) state.
- Use a factory or cache to reuse shared objects.

## When to use

- High object counts with identical configuration (e.g., glyphs, tiles, particles).

## Best practices

- Keep flyweights immutable so sharing is safe.
- Hide caching inside a factory function or class.

## Example

```python
from dataclasses import dataclass


@dataclass(frozen=True)
class GlyphShape:
    char: str
    font: str


_cache: dict[tuple[str, str], GlyphShape] = {}


def get_glyph(char: str, font: str) -> GlyphShape:
    key = (char, font)
    if key not in _cache:
        _cache[key] = GlyphShape(char, font)
    return _cache[key]


class GlyphInstance:
    def __init__(self, shape: GlyphShape, x: int, y: int) -> None:
        self.shape = shape  # shared
        self.x = x          # extrinsic
        self.y = y          # extrinsic


instances = [GlyphInstance(get_glyph("A", "Arial"), x, 0) for x in range(1000)]
print(len({id(g.shape) for g in instances}))  # 1 shared shape
```
