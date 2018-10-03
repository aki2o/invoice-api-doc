### 取引先の更新
```
  PATCH /api/v1/partners/:id
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
curl -i -H "Authorization: BEARER [ACCESS_TOKEN]" -H "Content-Type: application/json" -d '{ "partner" : { "name" : "サンプル取引先2" }}' -X PATCH https://invoice.moneyforward.com/api/v1/partners/ABCDEFGHIJKLMNOPQRST123
```

#### レスポンス
```
HTTP/1.1 200 OK

{
  "id" : "ABCDEFGHIJKLMNOPQRST123",
  "code" : "ABCD-00001",
  "name" : "サンプル取引先2",
  "name_kana" : "サンプルトリヒキサキ",
  "name_suffix" : "様",
  "memo" : "",
  "created_at" : "2015/10/31T00:00:00.000+09:00",
  "updated_at" : "2015/10/31T00:00:00.000+09:00",
  "departments" : [
    {
      "id" : "ABCDEFGHIJKLMNOPQRST012",
      "name" : "開発部",
      "zip" : "123-4567",
      "tel" : "03-1234-5678",
      "prefecture" : "東京都",
      "address1" : "港区サンプル1-2-3",
      "address2" : "サンプルビル",
      "person_title" : "部長",
      "person_name" : "サンプル太郎",
      "email" : "sample@example.com",
      "cc_emails" : "sample@example.com, sample2@example.com",
      "created_at" : "2015/10/31T00:00:00.000+09:00",
      "updated_at" : "2015/10/31T00:00:00.000+09:00"
    }
  ]
}
```

#### エラーレスポンス
```
HTTP/1.1 422 Unprocessable Entity

{
  "code" : "422",
  "errors" : [
    {
      "message" : "バリデーションに失敗しました。 名称を入力してください。"
    }
  ]
}
```
