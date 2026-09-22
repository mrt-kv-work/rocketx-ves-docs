# Venezuela / VES API

Venezuela payment API reference for VES orders and withdraws.

## Contents

- [Signature](./guides/signature.md)
- [Order](./guides/order.md)
- [Withdraw](./guides/withdraw.md)
- [Statuses](./guides/statuses.md)
- [Errors](./guides/errors.md)
- [Callback](./guides/callback.md)
- [API Reference](./reference.html ':ignore')

## Base Flow

Use the VES section when the operation currency is `VES`.

Supported operation types:

- Order: the customer sends a payment to returned requisites.
- Withdraw: RocketX processes a payout to the customer requisites.

Available payment methods and limits depend on the merchant configuration.
