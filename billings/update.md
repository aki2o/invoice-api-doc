### 請求書の更新
```
  PATCH /api/v2/billings/:id
```
#### パラメーター
| 名称         | field                        | 備考 |
| :--          | :--                          | :-- |
| 部門ID       | billing[department_id]       | |
| 件名         | billing[title]               | |
| 請求書番号   | billing[billing_number]      | |
| 振込先       | billing[payment_condition]   | |
| 備考         | billing[note]                | |
| 請求日       | billing[billing_date]        | |
| お支払期限   | billing[due_date]            | |
| 売上計上日   | billing[sales_date]          | |
| メモ         | billing[memo]                | |
| 帳票名       | billing[document_name]       | |
| タグ         | billing[tags]                | カンマ区切り文字列で記載 |
| 品目1 名前   | billing[items][0][name]       | |
| 品目1 コード | billing[items][0][code]       | |
| 品目1 詳細   | billing[items][0][detail]     | |
| 品目1 数量   | billing[items][0][quantity]   | |
| 品目1 単価   | billing[items][0][unit_price] | |
| 品目1 単位   | billing[items][0][unit]       | |
| 品目1 税対象 | billing[items][0][excise]     | 0: false  1: true |
| 品目2 名前   | billing[items][1][name]       | |
| 品目2 コード | billing[items][1][code]       | |
| 品目2 詳細   | billing[items][1][detail]     | |
| 品目2 数量   | billing[items][1][quantity]   | |
| 品目2 単価   | billing[items][1][unit_price] | |
| 品目2 単位   | billing[items][1][unit]       | |
| 品目2 税対象 | billing[items][1][excise]     | 0: false  1: true |

#### リクエスト例
```
curl -i -H "Authorization: BEARER [ACCESS_TOKEN]" -H "Content-Type: application/json" -d '{ "billing" : { "title" : "件名サンプル" }}' -X PATCH https://invoice.moneyforward.com/api/v2/billings/ABCDEFGHIJKLMNOPQRST123

# 品目を含むリクエスト
curl -i -H "Authorization: BEARER [ACCESS_TOKEN]" -H "Content-Type: application/json" \
-d '
{
  "billing" : {
    "department_id" : "DEPARTMENT_ID",
      "items" : [
        # itemの追加(idなし)
        {
          "name" : "商品C",
          "quantity" : "1",
          "unit_price" : "100"
        },
        # itemの更新(idあり)
        {
          "id" : "ABCDEFGHIJKLMNOPQRST012",
          "name" : "商品B",
          "quantity" : "1",
          "unit_price" : "100"
        },
        # itemの更新(削除)
        {
          "id" : "ABCDEFGHIJKLMNOPQRST012",
          "_destroy" : true
        },
      ]
  }
} '
-X PATCH https://invoice.moneyforward.com/api/v2/billings/"更新する請求書ID"
```

#### レスポンス

HTTP/1.1 200 OK

[billing.json](./responses/billing.json)

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
