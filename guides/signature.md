# Venezuela / VES: Signature

All VES API requests must be signed.

## Required Headers

| Header | Required | Description |
|---|:---:|---|
| `PublicKey` | Yes | Partner public key issued by RocketX. |
| `Signature` | Yes | Request signature generated with the partner private key. |
| `Timestamp` | Yes | Current Unix timestamp in UTC seconds. |
| `Content-Type` | Yes | Use `application/json` for JSON requests. |

Requests with an invalid key, invalid signature, or expired timestamp return
HTTP `401`.

Generate a fresh timestamp for every request. A timestamp 30 seconds or more
behind the server time is rejected. Keep your system clock synchronized.

## Signature Formula

```text
Signature = HMAC-SHA512(private_key, Timestamp + raw_request_body)
```

Sign the exact JSON body that is sent to the API. Do not reformat, sort, trim,
or otherwise transform the body before generating the signature.

## Headers Example

```http
Content-Type: application/json
PublicKey: merchant-public-key
Timestamp: 1754549516
Signature: generated-hmac-sha512-hex-string
```

Do not send the private key in requests.

Send the signature as a lowercase hex string. The example timestamp is
illustrative; use the current Unix timestamp for an actual request.
