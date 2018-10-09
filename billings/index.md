### 請求書一覧の取得
```
  GET /api/v1/billings.json
```

#### パラメーター
| 名称                  | field    | 備考 |
| :--                   | :--      | :--|
| ページ番号            | page     | |
| 1ページあたりの表示数 | per_page | 100件まで |

#### リクエスト例
```
curl -i -H "Authorization: BEARER [ACCESS_TOKEN]" "https://invoice.moneyforward.com/api/v1/billings.json?page=1&per_page=10"
```

#### レスポンス
HTTP/1.1 200 OK

[billings.json](./responses/billings.json)
