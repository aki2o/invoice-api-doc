### 請求書の取得
```
  GET /api/v1/billings/:id.json
```

#### パラメーター
なし

#### リクエスト例
```
curl -i -H "Authorization: BEARER [ACCESS_TOKEN]" https://invoice.moneyforward.com/api/v1/billings/ABCDEFGHIJKLMNOPQRST123.json
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
  "payment_condition" : "",
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
      "name" : "商品A",
      "code" : "ITEM-001",
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
```
{
  "code" : "404",
  "errors" : [
    {
      "message" : "存在しないIDが渡されました。"
    }
  ]
}
```

### 請求書pdfの取得
```
  GET /api/v1/billings/:id.pdf
```

#### パラメーター
なし

#### リクエスト例
```
curl -i -H "Authorization: BEARER [ACCESS_TOKEN]" https://invoice.moneyforward.com/api/v1/billings/ABCDEFGHIJKLMNOPQRST123.pdf -O
```

#### レスポンス
pdfファイルがバイナリで送信されます。
```
HTTP/1.1 200 OK
Content-Type: application/pdf
```

#### エラーレスポンス
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
