### 請求書の入金ステータスの更新
```
  PATCH /api/v1/billings/:id/billing_status/payment
```
#### パラメーター
| 名称           | field                   | 必須 or 任意 | 備考                                        |
| :--            | :--                     | :--          | :--                                         |
| 入金ステータス | billing_status[payment] | 必須         | '0': 未設定, '1': '未入金', '2': '入金済み' |

#### リクエスト例
```
curl -i -H "Authorization: BEARER [ACCESS_TOKEN]" -H "Content-Type: application/json" -d '{ "billing_status" : { "payment" : "0"} }' -X PATCH https://invoice.moneyforward.com/api/v1/billings/ABCDEFGHIJKLMNOPQRST123/billing_status/payment
```

#### レスポンス
```
HTTP/1.1 200 OK

{
  "id" : "ABCDEFGHIJKLMNOPQRST123",
  "pdf_url" : "https://invoice.moneyforward.co.jp/api/v1/billings/:id.pdf",
  "user_id" : "ABCDEFGHIJKLMNOPQRST456",
  "partner_id" : "ABCDEFGHIJKLMNOPQRST789",
  "department_id" : "ABCDEFGHIJKLMNOPQRST012",
  "partner_name" : "サンプル取引先",
  "partner_name_suffix" : "様",
  "partner_detail" : "hogehoge",
  "member_id" : "ABCDEFGHIJKLMNOPQRST345",
  "member_name" : "担当者A",
  "office_name" : "サンプル事業所",
  "office_detail" : "",
  "title" : "件名サンプル",
  "excise_price" : 80,
  "deduct_price" : 0,
  "subtotal" : 1000,
  "memo" : "",
  "payment_condition" : "",
  "total_price" : 1080,
  "billing_date" : "2015/10/31",
  "due_date" : "2015/11/30",
  "sales_date" : "2015/10/31",
  "created_at" : "2015/10/31T00:00:00.000+09:00",
  "updated_at" : "2015/10/31T00:00:00.000+09:00",
  "billing_number" : "1",
  "note" : "",
  "document_name" : "",
  "tags": [
    "tag1",
    "tag2"
  ],
  "status" : {
    "posting" : "未郵送",
    "email" : "未送信",
    "download" : "",
    "payment" : "未設定"
  },
  "items" : [
    {
      "id" : "ABCDEFGHIJKLMNOPQRST012",
      "code" : "ITEM-001",
      "name" : "商品A",
      "detail" : "",
      "quantity" : 1,
      "unit_price" : 1000,
      "unit" : "個",
      "price" : 1000,
      "excise": true,
      "display_order" : 0,
      "created_at" : "2015/10/31T00:00:00.000+09:00",
      "updated_at" : "2015/10/31T00:00:00.000+09:00"
    }
  ]
}
```

#### エラーレスポンス
IDが不正である場合
```
HTTP/1.1 404 Not Found

{
  "code" : "404",
  "errors" : [
    {
      "message" : "存在しないIDが渡されました。"
    }
  ]
}
```

不正な帳票を指定した場合
```
HTTP/1.1 400 Bad Request

{
  "code" : "400",
  "errors" : [
    {
      "message" : "受け取った帳票のステータスは変更できません。"
    }
  ]
}
```

不正なパラメータを指定した場合
```
HTTP/1.1 400 Bad Request

{
  "code" : "400",
  "errors" : [
    {
      "message" : "ステータスの値が不正です。"
    }
  ]
}
```
