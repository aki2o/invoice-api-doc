### 品目の取得
```
  GET /api/v1/items/:id.json
```

#### パラメーター
なし

#### リクエスト例
```
curl -i -H "Authorization: BEARER [ACCESS_TOKEN]" https://invoice.moneyforward.com/api/v1/items/ABCDEFGHIJKLMNOPQRST123.json
```

#### レスポンス
HTTP/1.1 200 OK

```
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
```
