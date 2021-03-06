### 見積書作成

```
POST   /api/v2/quotes.json
```

#### detail


| プラン     | 見積書作成リクエスト上限 |
| :--        | --:                      |
| フリー     | 月100件まで              |
| ベーシック | 月100件まで              |
| プロ       | なし                     |

[プラン体系変更](https://support.biz.moneyforward.com/valuepack/news/important/i000.html)後のリクエスト上限は下記になります。

| 事業所区分 | プラン           | 見積書作成リクエスト上限 |
| :--        | :--              | --:                      |
|            | フリー           | 月100件まで              |
| 個人       | パーソナルライト | 月100件まで              |
| 個人       | パーソナル       | なし                     |
| 個人       | パーソナルプラス | なし                     |
| 法人       | スモールビジネス | 月100件まで              |
| 法人       | ビジネス         | なし                     |

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
| quote[items][0][is_excise]  | bool |         |              | no   |                                |
| quote[items][0][code]       | str  |         |              | no   | 登録済の品目を使う場合に指定。 |

#### curl

```
curl -X POST "https://invoice.moneyforward.com/api/v2/quotes.json" -H "Authorization: BEARER {{TOKEN}}" -H "Content-Type: application/json" \
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

[quote.json](./responses/quote.json)

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
