### 送付履歴一覧の取得
```
  GET /api/v2/sent_histories.json
```

#### パラメーター
| 名称                  | field    | 備考 |
| :--                   | :--      | :--|
| ページ数              | page     | |
| 1ページあたりの表示数 | per_page | 100件まで |

#### リクエスト例
```
curl -i -H "Authorization: BEARER [ACCESS_TOKEN]" "https://invoice.moneyforward.com/api/v2/sent_histories.json?page=1&per_page=10"
```

#### レスポンス
HTTP/1.1 200 OK

[sent_histories.json](./responses/sent_histories.json)
