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
HTTP/1.1 200 OK

[item.json](./responses/item.json)
