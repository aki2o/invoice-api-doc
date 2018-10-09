### 事業所情報の編集
```
  PATCH /api/v1/office
```

#### パラメーター
| 名称     | field              | 備考 |
| :--      | :--                | :--|
| 名前     | office[name]       | |
| 郵便番号 | office[zip]        | |
| 都道府県 | office[prefecture] | |
| 住所1    | office[address1]   | |
| 住所2    | office[address2]   | |
| 電話番号 | office[tel]        | |
| FAX番号  | office[fax]        | |

### リクエスト例
```
curl -i -H "Authorization: BEARER [ACCESS_TOKEN]" -H "Content-Type: application/json" -d '{ "office" : {"name" : "サンプル事業所2"} }' -X PATCH https://invoice.moneyforward.com/api/v1/office
```

#### レスポンス

HTTP/1.1 200 OK

[office.json](./responses/office.json)
