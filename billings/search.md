### 請求書一覧の検索
```
  GET /api/v1/billings/search.json
```

#### パラメーター
| 名称                  | field     | 備考 |
| :--                   | :--       | :-- |
| ページ番号            | page      | |
| 1ページあたりの表示数 | per_page  | 100件まで |
| 検索文字列            | q         |  取引先(完全一致), ステータス、件名etc |
| 期間絞込対象          | range_key |  created_at(デフォルト), sales_date, billing_dateのみ許可 |
| 期間開始日            | from      |  デフォルト: 月初 |
| 期間終了日            | to        |  デフォルト: 月末 |

#### リクエスト例
```
curl -i -H "Authorization: BEARER [ACCESS_TOKEN]" "https://invoice.moneyforward.com/api/v1/billings/search.json?q=hoge&range_key=created_at&from=2015-10-01&to=2015-10-31"
```

#### レスポンス
```
HTTP/1.1 200 OK

{
  "meta" : {
    "total_count" : 1,
    "total_pages" : 1,
    "current_page" : 1,
    "per_page" : 100,
    "condition" : {
      "query" : "hoge",
      "range_key" : "created_at",
      "from" : "2015-10-01",
      "to" : "2015-10-31"
    }
  },
  "billings" : [
    {
      "id" : "ABCDEFGHIJKLMNOPQRST123",
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
  ]
}
```
