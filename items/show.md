### 品目の取得
```
  GET /api/v2/items/:id.json
```

#### パラメーター
なし

#### リクエスト例
```
curl -i -H "Authorization: BEARER [ACCESS_TOKEN]" https://invoice.moneyforward.com/api/v2/items/ABCDEFGHIJKLMNOPQRST123.json
```

#### レスポンス
HTTP/1.1 200 OK

[item.json](./responses/item.json)
