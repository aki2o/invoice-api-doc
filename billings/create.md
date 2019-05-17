### 請求書の作成
```
  POST /api/v2/billings
```

#### パラメーター
| 名称         | field                        | 備考 |
| :--          | :--                          | :-- |
| 部門ID       | billing[department_id]       | 必須 |
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
curl -i -H "Authorization: BEARER [ACCESS_TOKEN]" -H "Content-Type: application/json" -d '{ "billing" : { "department_id" : "DEPARTMENT_ID" }}' -X POST https://invoice.moneyforward.com/api/v2/billings

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
-X POST https://invoice.moneyforward.com/api/v2/billings

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
-X POST https://invoice.moneyforward.com/api/v2/billings
```
#### レスポンス

HTTP/1.1 201 Created

[billing.json](./responses/billing.json)

#### エラーレスポンス
department_idが不正であった場合
```
{
  "errors": {
    "status": "404",
    "title": "No resource found",
    "detail": "存在しないIDが渡されました。"
  }
}
```
