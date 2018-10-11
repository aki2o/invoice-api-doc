### 取引先の更新
```
  PATCH /api/v2/partners/:id
```

#### パラメーター
| 名称             | field                                 | 備考 |
| :--              | :--                                   | :--|
| 顧客コード       | partner[code]                         | |
| 名前             | partner[name]                         | |
| 名前（カナ）     | partner[name_kana]                    | |
| 敬称             | partner[name_suffix]                  | |
| メモ             | partner[memo]                         | |
| 部門ID           | partner[departments][0][id]           | departmentの値を更新する場合は必須 |
| 郵便番号         | partner[departments][0][zip]          | |
| 電話番号         | partner[departments][0][tel]          | |
| 都道府県         | partner[departments][0][prefecture]   | |
| 住所1            | partner[departments][0][address1]     | |
| 住所2            | partner[departments][0][address2]     | |
| 担当者氏名       | partner[departments][0][person_name]  | |
| 担当者役職       | partner[departments][0][person_title] | |
| 部門名           | partner[departments][0][name]         | |
| メールアドレス   | partner[departments][0][email]        | |
| ccメールアドレス | partner[departments][0][cc_emails]    | |

#### リクエスト例
```
curl -i -H "Authorization: BEARER [ACCESS_TOKEN]" -H "Content-Type: application/json" -d '{ "partner" : { "name" : "サンプル取引先2" }}' -X PATCH https://invoice.moneyforward.com/api/v2/partners/ABCDEFGHIJKLMNOPQRST123
```

#### レスポンス
HTTP/1.1 200 OK

[partner.json](./responses/partner.json)

#### エラーレスポンス
```
HTTP/1.1 422 Unprocessable Entity

{
  "errors": {
    "status": "422",
    "title": "Unprocessable entity",
    "detail": "保存に失敗しました。 名称を入力してください。"
  }
}
```
