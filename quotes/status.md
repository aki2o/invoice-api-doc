### 見積書ステータス更新

```
PATCH  /api/v2/quotes/:quote_id/order_status.json
```

#### detail

なし

#### params

| key                 | type | default | constraints                                | must | description |
| :--                 | :--  | :--     | :--                                        | :--  | :--         |
| quote[order_status] | str  |         | (default, failure, not_received, received) | yes  |             |

#### curl

```
curl -X PATCH "https://invoice.moneyforward.com/api/v2/quotes/asdfghjkl/order_status.json" -H "Authorization: BEARER {{TOKEN}}" \
-d '
{
  "quote": {
    "order_status": "received"
  }
}
'
```

#### response
##### success
HTTP/1.1 200 OK

[quote.json](/responses/quote.json)

##### failure
HTTP/1.1 400 Bad Request

```
{
  "errors": [
    {
      "id": "status_value",
      "status": "400",
      "code": "170",
      "title": "Invalid attribute",
      "detail": "不正な値です。",
      "source": {
        "pointer": "/quote/order_status"
      }
    }
  ]
}
```
