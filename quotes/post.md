### 見積書の郵送

```
POST   /api/v2/quotes/:quote_id/posting.json
```

#### detail

なし

#### params

なし

#### curl

```
curl -X POST "https://invoice.moneyforward.com/api/v2/quotes/asdfghjkl/posting.json" -H "Authorization: BEARER {{TOKEN}}"
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
      "code": "150",
      "title": "Unprocessable entity",
      "detail": "既に郵送済みです"
    }
  ]
}
```

HTTP/1.1 422 Unprocessable Entity
```
{
  "errors": [
    {
      "status": "422",
      "code": "151",
      "title": "Unprocessable entity",
      "detail": "取引先部門に住所を設定してください。"
    }
  ]
}
```

HTTP/1.1 403 Forbidden

```
{
  "errors": [
    {
      "status": "403",
      "code": "152",
      "title": "Forbidden",
      "detail": "プランの登録が必要です"
    }
  ]
}
```
