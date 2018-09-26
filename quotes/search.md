### 見積書検索

```
GET    /api/v2/quotes/search.json
```

#### detail

なし

#### params

| key         | type | default    | constraints                            | must | description            |
| :--         | :--  | :--        | :--                                    | :--  | :--                    |
| q           | str  |            |                                        | no   |                        |
| range[key]  | str  | created_at | (created_at, quote_date, expired_date) | no   | 指定した期間で絞り込み |
| range[from] | date | 今月初     | 'yyyy-mm-dd', from <= to               | no   | 指定した期間で絞り込み |
| range[to]   | date | 今月末     | 'yyyy-mm-dd', from <= to               | no   | 指定した期間で絞り込み |
| per_page    | int  | 100        | 0 < val <= 100                         | no   | 取得する件数           |
| page        | int  | 1          | 0 < val                                | no   | 取得するページ         |

#### curl

```
curl "https://invoice.moneyforward.com/api/v2/quotes/search.json?q=hoge&range[key]=quote_date&range[from]=2018-09-11" -H "Authorization: BEARER {{TOKEN}}"
```

#### response
##### success
HTTP/1.1 200 OK

[quotes.json](/responses/quotes.json)

##### failure
HTTP/1.1 400 Bad Request

```
{
  "errors": [
    {
      "status": "400",
      "code": "210",
      "title": "Bad request",
      "detail": "per_page <= 100 である必要があります。"
    }
  ]
}
```

HTTP/1.1 400 Bad Request
```
{
  "errors": [
    {
      "status": "400",
      "code": "190",
      "title": "Bad request",
      "detail": "from <= to である必要があります。"
    }
  ]
}
```

HTTP/1.1 400 Bad Request
```
{
  "errors": [
    {
      "status": "400",
      "code": "190",
      "title": "Bad request",
      "detail": "invalid date"
    }
  ]
}
```
