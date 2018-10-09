### 請求書の入金ステータスの更新
```
  PATCH /api/v1/billings/:id/payment_status
```
#### パラメーター
| 名称           | field                   | 必須 or 任意 | 備考                                        |
| :--            | :--                     | :--          | :--                                         |
| 入金ステータス | billing[payment_status] | 必須         | '0': 未設定, '1': '未入金', '2': '入金済み' |

#### リクエスト例
```
curl -i -H "Authorization: BEARER [ACCESS_TOKEN]" -H "Content-Type: application/json" -d '{ "billing" : { "payment_status" : "0"} }' -X PATCH https://invoice.moneyforward.com/api/v1/billings/ABCDEFGHIJKLMNOPQRST123/payment_status
```

#### レスポンス
HTTP/1.1 200 OK

[billing.json](./responses/billing.json)

#### エラーレスポンス

不正な帳票を指定した場合
```
HTTP/1.1 400 Bad Request

{
  "errors": {
    "status": "400",
    "title": "Bad request",
    "detail": "受け取った帳票のステータスは変更できません。"
  }
}
```

不正なパラメータを指定した場合
```
HTTP/1.1 400 Bad Request

{
  "errors": {
    "status": "400",
    "title": "Bad request",
    "detail": "ステータスの値が不正です。"
  }
}
```
