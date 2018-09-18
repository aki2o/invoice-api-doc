### 見積書更新

```
PATCH  /api/v2/quotes/:id.json
```

#### detail

なし

#### params

| key                         | type | default | constraints  | must | description                                  |
| :--                         | :--  | :--     | :--          | :--  | :--                                          |
| quote[department_id]        | str  |         |              | yes  |                                              |
| quote[quote_number]         | str  |         |              | no   |                                              |
| quote[quote_date]           | date |         | 'yyyy-mm-dd' | no   |                                              |
| quote[expired_date]         | date |         | 'yyyy-mm-dd' | no   |                                              |
| quote[title]                | str  |         |              | no   |                                              |
| quote[note]                 | str  |         |              | no   |                                              |
| quote[memo]                 | str  |         |              | no   |                                              |
| quote[tags]                 | str  |         |              | no   | カンマ区切り。ex: 'hoge,fuga'                |
| quote[items][0][id]         | str  |         |              | no   | 指定するとidの品目を更新。指定しないと追加。 |
| quote[items][0][name]       | str  |         |              | no   |                                              |
| quote[items][0][unit_price] | int  |         |              | no   |                                              |
| quote[items][0][quantity]   | int  |         |              | no   |                                              |
| quote[items][0][detail]     | str  |         |              | no   |                                              |
| quote[items][0][excise]     | bool |         |              | no   |                                              |
| quote[items][0][code]       | str  |         |              | no   | 登録済の品目を追加する場合に指定。           |
| quote[items][0][_destroy]   | bool |         |              | no   | 品目を削除する場合指定。                     |

#### curl

```
curl -X PATCH "https://invoice.moneyforward.com/api/v2/quotes/asdfghjkl.json" -H "Authorization: BEARER {{TOKEN}}" \
-d '
{
  "quote": {
    "title": "新しいタイトル",
    "items": [
      {
        "id": "poiuytrewq",
        "name" : "新しい商品名B"
      },
      {
        "name" : "追加する商品名C"
      },
      {
        "id": "qwertyuiop",
        "_destroy" : "true"
      }
    ]
  }
}'
```

#### response
##### success
HTTP/1.1 200 OK

[quote.json](/responses/quote.json)

##### failure
HTTP/1.1 400 Bad Request

```
{
  "errors": [
    {
      "status": "400",
      "code": "120",
      "title": "Invalid attribute",
      "detail": "error detail",
      "source": {
        "pointer": "/quote/attr"
      }
    }
  ]
}
```
