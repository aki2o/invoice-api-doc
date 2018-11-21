### 品目の削除
```
  DELETE /api/v2/items/:id
```

#### パラメーター
なし

#### リクエスト例
```
curl -i -H "Authorization: BEARER [ACCESS_TOKEN]" -X DELETE https://invoice.moneyforward.com/api/v2/items/ABCDEFGHIJKLMNOPQRST123
```

#### レスポンス
```
HTTP/1.1 204 No Content
```
