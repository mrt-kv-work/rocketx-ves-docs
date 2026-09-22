# Venezuela / VES: Errors

VES API errors are returned as JSON except for empty `404` responses.

## HTTP Response Summary

| Request | Result | HTTP status |
|---|---|---:|
| Create order or withdraw | Successful request | `200` |
| Create order or withdraw | Validation or business error | `400` |
| Any API request | Invalid authentication | `401` |
| Status lookup | Missing or unknown operation identifier | `404` |
| Create order or withdraw | Unexpected processing error | `500` |

The structured `500` response described below is produced by the create
endpoints. Status lookup endpoints are not covered by that error handler.

## Authentication Error

Invalid authentication returns HTTP `401`.

```json
{
  "message": "Bad credentials"
}
```

This response can be returned when `PublicKey`, `Signature`, or `Timestamp` is
invalid.

## Validation Or Business Error

Validation and business errors return HTTP `400`.

This applies to the initial create request in both H2H and redirect
integrations. RocketX does not return `form_uri` in an error response, so the
partner must process the error before redirecting the customer.

```json
{
  "errors": {
    "payment_type": ["is invalid"]
  }
}
```

Common examples:

| Error | Meaning |
|---|---|
| `user_cid`: `can't be blank` | `user_cid` is required. |
| `cid`: `can't be blank` | Partner operation ID is required. |
| `cid`: `has already been taken` | Partner operation ID must be unique for the merchant. |
| `fingerprint`: `can't be blank` | Customer fingerprint is required. |
| `payment_type`: `is invalid` | Payment method is not available for the merchant, currency, or operation type. |
| `base`: `banned_client` | Customer is blocked. |
| `base`: `no_requisite` | No payment requisites are currently available for the order. |
| `base`: `AMOUNT_OUT_OF_LIMIT` | Order amount is outside the currently available processing limits. |
| `payment_type_id`: `can't be blank` | A payment method is required when withdraw `requisite` is provided. |
| `amount`: `must be greater than 0` | A positive amount is required when withdraw `requisite` is provided. |
| `requisite.phone`: invalid | Phone must use the `0XXXXXXXXXX` format and a supported operator code. |
| `requisite.document_number`: invalid | Document number must use the expected document format. |
| `requisite.bank`: invalid | Bank value is missing or not supported. |
| `requisite.fio`: invalid | Customer full name is missing, too long, or contains unsupported characters. |

## Internal Error

Unexpected errors while creating an order or withdraw return HTTP `500` with
the following body:

```json
{
  "errors": {
    "base": ["Internal error"]
  }
}
```

## Not Found

Status lookup for an unknown `order_id`, `withdraw_id`, or `cid` returns HTTP
`404` with an empty response body.

## Hosted Page Errors

After a successful redirect create request, validation or allocation errors on
the hosted page are displayed there for the customer to correct. They are not
returned as a second response to the original API request. Use callbacks or the
status lookup endpoint to track later operation status changes.
