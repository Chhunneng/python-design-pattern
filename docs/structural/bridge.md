# Bridge

Reference: `https://python-patterns.guide/gang-of-four/principles/#solution-2-the-bridge-pattern`

## What is it?

Split an abstraction from its implementation so they can vary independently. Two dimensions can now change without creating a subclass for each combination.

## Why use it?

- Avoids subclass explosion when two (or more) axes vary.
- Lets you swap implementations at runtime.

## How to use it

- Define an implementation interface (e.g., `Device`).
- Define an abstraction that holds a reference to the implementation (e.g., `Remote`).
- Delegate work from the abstraction to the implementation.

## When to use

- You see classes like `TVRemote`, `RadioRemote`, `AdvancedTVRemote`, `AdvancedRadioRemote` forming.

## Best practices

- Keep the abstraction small; forward calls to the implementation.
- Prefer composition over inheritance between the two parts.

## Example

```python
from abc import ABC, abstractmethod


class Device(ABC):
    @abstractmethod
    def power(self, on: bool) -> None: ...

    @abstractmethod
    def volume(self) -> int: ...

    @abstractmethod
    def set_volume(self, value: int) -> None: ...


class TV(Device):
    def __init__(self) -> None:
        self._on = False
        self._volume = 10

    def power(self, on: bool) -> None:
        self._on = on
        print(f"TV {'ON' if on else 'OFF'}")

    def volume(self) -> int:
        return self._volume

    def set_volume(self, value: int) -> None:
        self._volume = max(0, min(100, value))


class Remote:
    def __init__(self, device: Device) -> None:
        self.device = device

    def toggle_power(self) -> None:
        # Abstraction delegates to implementation
        self.device.power(not self.device.volume() < 0)  # simple demo

    def volume_up(self) -> None:
        self.device.set_volume(self.device.volume() + 5)

    def volume_down(self) -> None:
        self.device.set_volume(self.device.volume() - 5)


# Swap implementations without changing Remote
remote = Remote(TV())
remote.volume_up()
remote.volume_down()
```
