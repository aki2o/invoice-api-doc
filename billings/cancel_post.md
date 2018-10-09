### 請求書の郵送キャンセル
```
  DELETE /api/v1/billings/:id/posting
```

#### パラメーター
なし

#### リクエスト例
```
curl -i -H "Authorization: BEARER [ACCESS_TOKEN]" -H "Content-Type: application/json" -d '' -X DELETE https://invoice.moneyforward.com/api/v1/billings/ABCDEFGHIJKLMNOPQRST123/posting
```

#### レスポンス
```
HTTP/1.1 200 OK
```
