# Adapter

Reference: `https://python-patterns.guide/gang-of-four/principles/#solution-1-the-adapter-pattern`

## What is it?

Wrap a class with a different interface so it looks like the one your code expects. It “adapts” an incompatible API to a compatible one.

## Why use it?

- Integrate third‑party or legacy code without changing your app or the library.
- Keep your domain code clean from vendor‑specific details.

## How to use it

- Define the interface your app expects.
- Implement an adapter that translates calls to the target API.
- Inject/use the adapter where your code expects the interface.

## When to use

- You cannot change the third‑party/legacy API, but you can wrap it.
- Only a subset of the foreign API is needed.

## Best practices

- Keep adapters thin and mechanical — no business logic.
- Name them clearly: `XxxAdapter`.

## Example

```python
class PaymentGateway:
    def charge(self, cents: int, currency: str) -> None:
        raise NotImplementedError


# Third‑party client with a different API we cannot change
class VendorClient:
    def pay(self, amount_float: float, iso_currency: str) -> None:
        print(f"Paid {amount_float:.2f} {iso_currency} via vendor")


class VendorAdapter(PaymentGateway):
    def __init__(self, client: VendorClient) -> None:
        self.client = client

    def charge(self, cents: int, currency: str) -> None:
        amount = cents / 100.0
        self.client.pay(amount_float=amount, iso_currency=currency)


# Application code talks to PaymentGateway
gateway: PaymentGateway = VendorAdapter(VendorClient())
gateway.charge(2599, "USD")
```
