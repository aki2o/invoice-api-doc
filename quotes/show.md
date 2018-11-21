### 見積書取得

```
GET    /api/v2/quotes/:id.json
GET    /api/v2/quotes/:id.pdf
```

#### detail

なし

#### params

なし

#### curl

```
curl "https://invoice.moneyforward.com/api/v2/quotes/asdfghjkl.json" -H "Authorization: BEARER {{TOKEN}}"
curl "https://invoice.moneyforward.com/api/v2/quotes/asdfghjkl.pdf" -H "Authorization: BEARER {{TOKEN}}"
```

#### response
##### success
HTTP/1.1 200 OK

[quote.json](./responses/quote.json)

##### failure
```
HTTP/1.1 404 Not Found

{
  "errors": [
    {
      "status": "404",
      "code": "100",
      "title": "No resource found",
      "detail": "存在しないIDが渡されました。"
    }
  ]
}
```
