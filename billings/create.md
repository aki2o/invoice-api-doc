### 請求書の作成
```
  POST /api/v1/billings
```

#### パラメーター
| 名称         | field                      | 備考 |
| :--          | :--                        | :-- |
| 部門ID       | billing[department_id]     | 必須 |
| 件名         | billing[title]             | |
| 請求書番号   | billing[billing_number]    | |
| 振込先       | billing[payment_condition] | |
| 備考         | billing[note]              | |
| 請求日       | billing[billing_date]      | |
| お支払期限   | billing[due_date]          | |
| 売上計上日   | billing[sales_date]        | |
| メモ         | billing[memo]              | |
| 帳票名       | billing[document_name]     | |
| タグ         | billing[tags]              | カンマ区切り文字列で記載 |
| 品目1 名前   | items[0][name]             | |
| 品目1 コード | items[0][code]             | |
| 品目1 詳細   | items[0][detail]           | |
| 品目1 数量   | items[0][quantity]         | |
| 品目1 単価   | items[0][unit_price]       | |
| 品目1 単位   | items[0][unit]             | |
| 品目1 税対象 | items[0][excise]           | 0: false  1: true |
| 品目2 名前   | items[1][name]             | |
| 品目2 コード | items[1][code]             | |
| 品目2 詳細   | items[1][detail]           | |
| 品目2 数量   | items[1][quantity]         | |
| 品目2 単価   | items[1][unit_price]       | |
| 品目2 単位   | items[1][unit]             | |
| 品目2 税対象 | items[1][excise]           | 0: false  1: true |

#### リクエスト例
```
curl -i -H "Authorization: BEARER [ACCESS_TOKEN]" -H "Content-Type: application/json" -d '{ "billing" : { "department_id" : "DEPARTMENT_ID" }}' -X POST https://invoice.moneyforward.com/api/v1/billings

# 品目を含むリクエスト
curl -i -H "Authorization: BEARER [ACCESS_TOKEN]" -H "Content-Type: application/json" \
-d '
{
  "billing" : {
    "department_id" : "DEPARTMENT_ID",
      "items" : [
        {
          "name" : "商品A",
          "quantity" : "1",
          "unit_price" : "100"
        },
        # 登録した品目を利用する場合
        {
          "code" : "登録済み品目コード"
        }
      ]
  }
} '
-X POST https://invoice.moneyforward.com/api/v1/billings

# タグを含むリクエスト
curl -i -H "Authorization: BEARER [ACCESS_TOKEN]" -H "Content-Type: application/json" \
-d '
{
  "billing" : {
    "department_id" : "DEPARTMENT_ID",
      "items" : [
        {
          "name" : "商品A",
          "quantity" : "1",
          "unit_price" : "100"
        }
      ],
    "tags" : "tag1,tag2" # カンマ区切り文字列で入力
  }
} '
-X POST https://invoice.moneyforward.com/api/v1/billings
```
#### レスポンス
```
HTTP/1.1 201 Created

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
      "name" : "商品A",
      "detail" : "",
      "quantity" : 1,
      "unit_price" : 1000,
      "unit" : "個",
      "price" : 1000
      "excise": true,
      "display_order" : 0,
      "created_at" : "2015/10/31T00:00:00.000+09:00",
      "updated_at" : "2015/10/31T00:00:00.000+09:00"
    }
  ]
}
```

#### エラーレスポンス
department_idが不正であった場合
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