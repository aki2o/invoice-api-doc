### 品目の更新
```
  PATCH /api/v1/items/:id
```

#### パラメーター
| 名称               | field            | 備考 |
| :--                | :--              | :--|
| 名前               | item[name]       | |
| 品目コード         | item[code]       | |
| 詳細               | item[detail]     | |
| 単価               | item[unit_price] | |
| 単位               | item[unit]       | |
| 数量               | item[quantity]   | |
| 消費税を計算するか | item[excise]     | 0: false  1: true |

### リクエスト例
```
curl -i -H "Authorization: BEARER [ACCESS_TOKEN]" -H "Content-Type: application/json" -d '{ "item" : { "name" : "サンプル商品2" }}' -X PATCH https://invoice.moneyforward.com/api/v1/items/ABCDEFGHIJKLMNOPQRST123
```

#### レスポンス
```
HTTP/1.1 200 OK

{
  "id" : "ABCDEFGHIJKLMNOPQRST123",
  "name" : "サンプル商品2",
  "code" : "ITEM-001",
  "detail" : "",
  "unit_price" : 1000,
  "unit" : "個",
  "quantity" : 1,
  "price" : 1000,
  "excise" : true,
  "created_at" : "2015/10/31T00:00:00.000+09:00",
  "updated_at" : "2015/10/31T00:00:00.000+09:00"
}
```

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
