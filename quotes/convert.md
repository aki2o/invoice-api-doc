### 見積書を見積書に変換

```
POST   /api/v2/quotes/:quote_id/convert_to_billing.json
```

#### detail

なし

#### params

なし

#### curl

```
curl -X POST "https://invoice.moneyforward.com/api/v2/quotes/asdfghjkl/convert_to_billing.json" -H "Authorization: BEARER {{TOKEN}}"
```

#### response
##### success
HTTP/1.1 200 OK

```
{ data: { type: :billing, id: iv_billing.encrypted_id } }, status: :ok
```
