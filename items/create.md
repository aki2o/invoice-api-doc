### 品目の作成
```
  POST /api/v1/items
```

#### パラメーター
| 名称               | field            | 備考 |
| :--                | :--              | :--|
| 名前               | item[name]       | 必須 |
| 品目コード         | item[code]       | |
| 詳細               | item[detail]     | |
| 単価               | item[unit_price] | |
| 単位               | item[unit]       | |
| 数量               | item[quantity]   | |
| 消費税を計算するか | item[excise]     | 0: false  1: true |

#### リクエスト例
```
curl -i -H "Authorization: BEARER [ACCESS_TOKEN]" -H "Content-Type: application/json" -d '{ "item" : { "name" : "サンプル商品" }}' -X POST https://invoice.moneyforward.com/api/v1/items
```

#### レスポンス
```
HTTP/1.1 201 Created

{
  "id" : "ABCDEFGHIJKLMNOPQRST123",
  "name" : "サンプル商品",
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
```
{
  "code" : "402",
  "errors" : [
    {
      "message" : "名前を入力してください"
    }
  ]
}
```
