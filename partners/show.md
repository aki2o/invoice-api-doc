### 取引先の取得
```
  GET /api/v2/partners/:id.json
```

#### パラメーター
なし

### リクエスト例
```
curl -i -H "Authorization: BEARER [ACCESS_TOKEN]" https://invoice.moneyforward.com/api/v2/partners/ABCDEFGHIJKLMNOPQRST123.json
```

#### レスポンス
HTTP/1.1 200 OK

[partner.json](./responses/partner.json)
