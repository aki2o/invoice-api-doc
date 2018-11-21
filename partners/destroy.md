### 取引先の削除
```
  DELETE /api/v2/partners/:id
```

#### パラメーター
なし

#### リクエスト例
```
curl -i -H "Authorization: BEARER [ACCESS_TOKEN]" -X DELETE https://invoice.moneyforward.com/api/v2/partners/ABCDEFGHIJKLMNOPQRST123
```

#### レスポンス
```
HTTP/1.1 204 No Content
```
