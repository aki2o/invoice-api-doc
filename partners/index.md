### 取引先一覧の取得
```
  GET /api/v1/partners.json
```

#### パラメーター
| 名称                  | field    | 備考 |
| :--                   | :--      | :--|
| ページ数              | page     | |
| 1ページあたりの表示数 | per_page | 100件まで |

### リクエスト例
```
curl -i -H "Authorization: BEARER [ACCESS_TOKEN]" "https://invoice.moneyforward.com/api/v1/partners.json?page=1&per_page=10"
```

#### レスポンス
```
HTTP/1.1 200 OK

{
  "meta" : {
    "total_count" : 1,
    "total_pages" : 1,
    "current_page" : 1,
    "per_page" : 10,
  },
  "partners" : [
    {
      "id" : "ABCDEFGHIJKLMNOPQRST123",
      "code" : "ABCD-00001",
      "name" : "サンプル取引先",
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
          "cc_emails" : "sample@example.com, sample2@example.com"
          "created_at" : "2015/10/31T00:00:00.000+09:00",
          "updated_at" : "2015/10/31T00:00:00.000+09:00"
        }
      ]
    }
  ]
}
```
