### 見積書作成

```
POST   /api/v2/quotes.json
```

#### detail


| プラン     | 請求書作成リクエスト上限 |
| :--        | --:                      |
| Free       | 月100件まで              |
| Basic      | 月100件まで              |
| Pro        | なし                     |
| Enterprise | なし                     |

#### params

| key                         | type | default | constraints  | must | description                    |
| :--                         | :--  | :--     | :--          | :--  | :--                            |
| quote[department_id]        | str  |         |              | yes  |                                |
| quote[quote_number]         | str  |         |              | no   |                                |
| quote[quote_date]           | date |         | 'yyyy-mm-dd' | no   |                                |
| quote[expired_date]         | date |         | 'yyyy-mm-dd' | no   |                                |
| quote[title]                | str  |         |              | no   |                                |
| quote[note]                 | str  |         |              | no   |                                |
| quote[memo]                 | str  |         |              | no   |                                |
| quote[tags]                 | str  |         |              | no   | カンマ区切り。ex: 'hoge,fuga'  |
| quote[items][0][name]       | str  |         |              | no   |                                |
| quote[items][0][unit_price] | int  |         |              | no   |                                |
| quote[items][0][quantity]   | int  |         |              | no   |                                |
| quote[items][0][detail]     | str  |         |              | no   |                                |
| quote[items][0][excise]     | bool |         |              | no   |                                |
| quote[items][0][code]       | str  |         |              | no   | 登録済の品目を使う場合に指定。 |

#### curl

```
curl -X POST "https://invoice.moneyforward.com/api/v2/quotes.json" -H "Authorization: BEARER {{TOKEN}}" \
-d '
{
  "quote": {
    "department_id": "asdfghjkl",
    "items": [
      {
        "name" : "商品A",
        "quantity" : 1,
        "unit_price" : 100
      },
      {
        "code": "qwertyuiop"
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
