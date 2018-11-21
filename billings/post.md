### 請求書の郵送
```
  POST /api/v2/billings/:id/posting
```

#### パラメーター
なし

#### リクエスト例
```
curl -i -H "Authorization: BEARER [ACCESS_TOKEN]" -H "Content-Type: application/json" -d '' -X POST https://invoice.moneyforward.com/api/v2/billings/ABCDEFGHIJKLMNOPQRST123/posting
```

#### レスポンス
```
HTTP/1.1 200 OK
```

#### エラーレスポンス
既に郵送済みである場合
```
HTTP/1.1 400 Bad Request

{
  "errors": {
    "status": "400",
    "title": "Bad request",
    "detail": "既に郵送済みです"
  }
}
```
