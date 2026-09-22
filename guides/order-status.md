# Venezuela / VES: Get Order Status

```http
POST /eapi/orders/get
Content-Type: application/json
```

Find the order by RocketX `order_id` or by partner `cid`.

```json
{
  "order_id": "430551eecdb8ecb842621cce"
}
```

The response contains the full order data, including `status`, `currency`,
`payment_type`, amounts, commission, timestamps, partner IDs, customer fields,
and `form_options`.

### Response Example

Illustrative amounts, rates and timestamps; actual values depend on the operation and merchant configuration.

```json
{
  "order_id": "430551eecdb8ecb842621cce",
  "cid": "order-128",
  "status": "in_work",
  "canceled_reason": "",
  "timer": 1788603300,
  "currency": "VES",
  "payment_type": "VESVES",
  "amount_in_currency": 100.55,
  "rate": 1,
  "full_amount": 100.55,
  "commission": 0,
  "amount": 100.55,
  "created_at": "2026-09-05T10:00:00.000Z",
  "updated_at": "2026-09-05T10:05:00.000Z",
  "description": "Order description",
  "merchant_cid": "merchant-2",
  "user_cid": "client-1",
  "fio": "Juan Garcia",
  "dni": null,
  "fingerprint": "customer-fingerprint",
  "callback_url": "https://merchant.example/callbacks/orders",
  "form_options": {
    "redirect_url": "https://merchant.example/orders/order-128"
  }
}
```

## Optional Customer Account Fields

When enabled for the merchant and supplied by the payment provider, the response
can also include `client_cvu` (customer account number) and `client_cuit`
(customer tax ID). Both fields are omitted if neither value is available.
If only one value is available, the other field is `null`.
