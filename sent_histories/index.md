### 送付履歴一覧の取得
```
  GET /api/v1/sent_history.json
```

#### パラメーター
| 名称                  | field    | 備考 |
| :--                   | :--      | :--|
| ページ数              | page     | |
| 1ページあたりの表示数 | per_page | 100件まで |

#### リクエスト例
```
curl -i -H "Authorization: BEARER [ACCESS_TOKEN]" "https://invoice.moneyforward.com/api/v1/sent_history.json?page=1&per_page=10"
```

#### レスポンス
HTTP/1.1 200 OK

```
{
  "meta": {
    "total_count" : 1,
    "total_pages" : 1,
    "current_page" : 1,
    "per_page" : 10
  },
  "sent_history_list": [
    {
      "operator_id":  "ABCDEFGHIJKLMNOP",
      "type": "メール",
      "document_type": "請求書",
      "document_id": "ABCDEFGHIJKLMNOP",
      "from": "",
      "to": "sample@moneyforward.co.jp",
      "cc": "",
      "sent_at": "2015-05-15T11:40:44.000+09:00"
    }
  ]
}
```
