### 請求書の郵送キャンセル
```
  POST /api/v1/billings/:id/cancel_posting
```

#### パラメーター
なし

#### リクエスト例
```
curl -i -H "Authorization: BEARER [ACCESS_TOKEN]" -H "Content-Type: application/json" -d '' -X POST https://invoice.moneyforward.com/api/v1/billings/ABCDEFGHIJKLMNOPQRST123/cancel_posting
```

#### レスポンス
```
HTTP/1.1 200 OK
```
