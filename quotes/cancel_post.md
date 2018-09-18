### 見積書の郵送取り消し

```
DELETE /api/v2/quotes/:quote_id/posting.json
```

#### detail

なし

#### params

なし

#### curl

```
curl -X DELETE "https://invoice.moneyforward.com/api/v2/quotes/asdfghjkl/posting.json" -H "Authorization: BEARER {{TOKEN}}"
```

#### response
##### success
HTTP/1.1 200 OK

(headers only)

##### failure
HTTP/1.1 422 Unprocessable Entity

```
{
  "errors": [
    {
      "status": "422",
      "code": "160",
      "title": "Unprocessable entity",
      "detail": "キャンセルの有効期限が過ぎています。"
    }
  ]
}
```
