# Composition Over Inheritance

Reference: `https://python-patterns.guide/gang-of-four/principles/`

## What is it?

Prefer building objects by combining small, focused components (composition) over extending large class hierarchies (inheritance). Composition keeps code flexible and avoids a “subclass explosion.”

## Why use it?

- Reduces tight coupling to parent classes.
- Lets you mix behaviors at runtime instead of hard‑coding them in a class tree.
- Keeps units small and testable.

## How to use it (in Python)

- Put behaviors into small classes (a.k.a. strategies, services, collaborators).
- Inject these collaborators into your main object via the constructor or properties.
- Delegate work to the collaborators instead of overriding methods in subclasses.

## When to use

- Requirements change often and you need to swap behaviors.
- You feel tempted to add “just one more subclass” for every new variation.
- Multiple dimensions vary (e.g., how to validate AND how to store data).

## Best practices

- Favor dependency injection: pass collaborators in, don’t create them inside.
- Keep components small and single‑purpose.
- Use protocols/ABCs or documented duck types for collaborator interfaces.

## Example

Problem: We log messages either to the console or to a file, with optional JSON formatting. Using inheritance for every combination (`ConsoleLogger`, `FileLogger`, `JsonConsoleLogger`, `JsonFileLogger`) doesn’t scale.

Solution: Compose a `Logger` with a `Sink` (where to write) and a `Formatter` (how to format).

```python
from abc import ABC, abstractmethod
from datetime import datetime


class Sink(ABC):
    @abstractmethod
    def write(self, text: str) -> None: ...


class ConsoleSink(Sink):
    def write(self, text: str) -> None:
        print(text)


class FileSink(Sink):
    def __init__(self, path: str) -> None:
        self.path = path

    def write(self, text: str) -> None:
        with open(self.path, "a", encoding="utf-8") as f:
            f.write(text + "\n")


class Formatter(ABC):
    @abstractmethod
    def format(self, level: str, message: str) -> str: ...


class SimpleFormatter(Formatter):
    def format(self, level: str, message: str) -> str:
        return f"[{level}] {message}"


class JsonFormatter(Formatter):
    def format(self, level: str, message: str) -> str:
        ts = datetime.utcnow().isoformat()
        return f'{{"ts":"{ts}","level":"{level}","message":"{message}"}}'


class Logger:
    def __init__(self, sink: Sink, formatter: Formatter) -> None:
        self.sink = sink
        self.formatter = formatter

    def log(self, level: str, message: str) -> None:
        self.sink.write(self.formatter.format(level, message))


# Compose at runtime
console_json = Logger(ConsoleSink(), JsonFormatter())
console_json.log("INFO", "hello")

file_simple = Logger(FileSink("app.log"), SimpleFormatter())
file_simple.log("ERROR", "something happened")
```

Thinking checklist:

- Can I swap this behavior without subclassing?
- Does this class do too many jobs that could be separate collaborators?
- Are variations multiplying across multiple dimensions?
