### 見積書一覧の取得

```
GET    /api/v2/quotes.json
```

#### detail

なし

#### params

| key      | type | default | constraints    | must | description    |
| :--      | :--  | :--     | :--            | :--  | :--            |
| per_page | int  | 100     | 0 < val <= 100 | no   | 取得する件数   |
| page     | int  | 1       | 0 < val        | no   | 取得するページ |

#### curl

```
curl "https://invoice.moneyforward.com/api/v2/quotes.json?page=2&per_page=10" -H "Authorization: BEARER {{TOKEN}}"
```

#### response
##### success
HTTP/1.1 200 OK

[quotes.json](/responses/quotes.json)

##### failure
```
TODO: 頑張る
```
