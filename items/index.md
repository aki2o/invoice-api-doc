### 品目一覧の取得
```
  GET /api/v1/items.json
```

#### パラメーター
| 名称                  | field    | 備考 |
| :--                   | :--      | :--|
| ページ数              | page     | |
| 1ページあたりの表示数 | per_page | 100件まで |

#### リクエスト例
```
curl -i -H "Authorization: BEARER [ACCESS_TOKEN]" "https://invoice.moneyforward.com/api/v1/items.json?page=1&per_page=10"
```

#### レスポンス
HTTP/1.1 200 OK

```
{
  "meta" : {
    "total_count" : 1,
    "total_pages" : 1,
    "current_page" : 1,
    "per_page" : 10
  },
  "items": [
    {
      "id" : "ABCDEFGHIJKLMNOPQRST123",
      "code" : "ITEM-001",
      "name" : "サンプル商品",
      "detail" : "",
      "unit_price" : 1000,
      "unit" : "個",
      "quantity" : 1,
      "price" : 1000,
      "excise" : true,
      "created_at" : "2015/10/31T00:00:00.000+09:00",
      "updated_at" : "2015/10/31T00:00:00.000+09:00"
    }
  ]
}
```
