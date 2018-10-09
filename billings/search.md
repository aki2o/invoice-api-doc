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
HTTP/1.1 200 OK

[billings.json](./responses/billings.json)
