### 請求書の取得
```
  GET /api/v2/billings/:id.json
  GET /api/v2/billings/:id.pdf
```

#### パラメーター
なし

#### リクエスト例
```
curl -i -H "Authorization: BEARER [ACCESS_TOKEN]" https://invoice.moneyforward.com/api/v2/billings/ABCDEFGHIJKLMNOPQRST123.json
```

#### レスポンス

HTTP/1.1 200 OK

[billing.json](./responses/billing.json)
