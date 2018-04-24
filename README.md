# MFクラウド請求書APIドキュメント

## プランごとの利用制限について
どのプランでも下記制限の中でAPIを利用することができます。

| プラン     | 請求書作成リクエスト上限 |
| :--        | --:                      |
| Free       | 100                      |
| Basic      | 100                      |
| Pro        | なし                     |
| Enterprise | なし                     |

## 認証について
アプリケーションの認証はOAuth2.0のAuthorization Code Grantにもとづいて行います。

### アプリケーションの登録
* 画面右上の歯車のアイコンをクリックし、そこからAPI連携を選びます
* 新規作成ボタンをクリックし、フォームに必要な情報を入力し、利用規約に同意する、にチェックを入れて作成ボタンをクリックします
* Client IDとClient Secretが発行されます
 redirect_uri は https のみ許可しています。

### アクセストークンの発行
* 前項で発行した Client IDとアプリケーション作成時に入力した値を使い、下記のようなURLにアクセスします。
```
https://invoice.moneyforward.com/oauth/authorize?client_id=[CLIENT_ID]&redirect_uri=[REDIRECT_URL]&response_type=code&scope=[SCOPE]
```
* アプリケーションを承認すると認証コードつきURLが発行されるので、そのコードを使って以下のようなリクエストをサーバーに発行し、アクセストークンを取得します。入力に使う[REDIRECT_URL]はアプリケーションの作成時に入力した値を入れてください

```
curl -d client_id=[CLIENT_ID] -d client_secret=[CLIENT_SECRET] -d redirect_uri=[REDIRECT_URL] -d grant_type=authorization_code -d code=[認証コード] -X POST https://invoice.moneyforward.com/oauth/token
```

※リクエストを送る際、下記2点について注意して下さい。

* パラメータは `Content-Type: application/x-www-form-urlencoded` にする
* クライアントによっては `Accept: application/json` ヘッダーの明示が必要

## アクセス数の制限について
* Basicプラン以下のプランをご利用の場合、月のAPI経由での請求書作成数を100件までとさせていただきます。
* その他、取引先登録数や郵送の可否等は通常のプランごとの制限と同様* となります。

## クライアントライブラリ

* [Ruby](https://github.com/moneyforward/mf_cloud-invoice-ruby)

## 事業所情報API
### エンドポイント
```
  /api/v1/office
```
### 事業所情報の取得

```
  GET /api/v1/office.json
```

#### パラメーター
なし

### リクエスト例
```
curl -i -H "Authorization: BEARER [ACCESS_TOKEN]" https://invoice.moneyforward.com/api/v1/office.json
```

#### レスポンス
```
HTTP/1.1 200 OK
{
  "name" : "サンプル事業所",
  "zip" : "123-4567",
  "prefecture" : "東京都",
  "address1" : "港区サンプル1-2-3",
  "address2" : "サンプルビル",
  "tel" : "03-1234-5678",
  "fax" : "03-5678-1234"
}
```

#### エラーレスポンス
アクセストークンが不正な場合
```
HTTP/1.1 401 Unauthorized
```

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
```
HTTP/1.1 200 OK

{
  "name" : "サンプル事業所2",
  "zip" : "123-4567",
  "prefecture" : "東京都",
  "address1" : "港区サンプル1-2-3",
  "address2" : "サンプルビル",
  "tel" : "03-1234-5678",
  "fax" : "03-5678-1234"
}
```

#### エラーレスポンス
パラメータ指定方法が誤っている場合
```
HTTP/1.1 400 Bad Request

{
  "code" : "400",
  "errors" : [
    {
      "message" : "必要なパラメーターが存在しない、もしくは空です。"
    }
  ]
}
```

不正な値があった場合
```
HTTP/1.1 400 Bad Request

{
  "code" : "400",
  "errors" : [
    {
      "message" : "不正な都道府県名が渡されました。"
    }
  ]
}
```

## 取引先API
### エンドポイント
```
  /api/v1/partners
```

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

### 取引先の取得
```
  GET /api/v1/partners/:id.json
```

#### パラメーター
なし

### リクエスト例
```
curl -i -H "Authorization: BEARER [ACCESS_TOKEN]" https://invoice.moneyforward.com/api/v1/partners/ABCDEFGHIJKLMNOPQRST123.json
```

#### レスポンス
```
HTTP/1.1 200 OK

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
      "cc_emails" : "sample@example.com, sample2@example.com",
      "created_at" : "2015/10/31T00:00:00.000+09:00",
      "updated_at" : "2015/10/31T00:00:00.000+09:00"
    }
  ]
}
```

### 取引先の作成
```
  POST /api/v1/partners
```

#### パラメーター
| 名称             | field                    | 備考 |
| :--              | :--                      | :--|
| 顧客コード       | partner[code]            | |
| 名前             | partner[name]            | 必須 |
| 名前（カナ）     | partner[name_kana]       | |
| 敬称             | partner[name_suffix]     | |
| メモ             | partner[memo]            | | |
| 郵便番号         | partner[zip]             | |
| 電話番号         | partner[tel]             | |
| 都道府県         | partner[prefecture]      | 入力なしの場合、東京都 |
| 住所1            | partner[address1]        | |
| 住所2            | partner[address2]        | |
| 担当者氏名       | partner[person_name]     | |
| 担当者役職       | partner[person_title]    | |
| 部門名           | partner[department_name] | |
| メールアドレス   | partner[email]           | |
| ccメールアドレス | partner[cc_emails]       | カンマ区切りで複数入力可 |

#### リクエスト例
```
curl -i -H "Authorization: BEARER [ACCESS_TOKEN]" -H "Content-Type: application/json" -d '{ "partner" : { "name" : "サンプル取引先", "code" : "ABCD-00001" }}' -X POST https://invoice.moneyforward.com/api/v1/partners
```

#### レスポンス
※取引先を作成すると、自動で部門(department)が作成されます。請求書作成時などではdepartment_のidを使用します。
```
HTTP/1.1 201 Created

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

### 取引先の削除
```
  DELETE /api/v1/partners/:id
```

#### パラメーター
なし

#### リクエスト例
```
curl -i -H "Authorization: BEARER [ACCESS_TOKEN]" -X DELETE https://invoice.moneyforward.com/api/v1/partners/ABCDEFGHIJKLMNOPQRST123
```

#### レスポンス
```
HTTP/1.1 204 No Content
```

#### エラーレスポンス
IDが不正である場合
```
HTTP/1.1 404 Not Found

{
  "code" : "404",
  "errors" : [
    {
      "message" : "存在しないIDが渡されました。"
    }
  ]
}
```

## 請求書API
### エンドポイント
```
  /api/v1/billings
```

### 請求書一覧の取得
```
  GET /api/v1/billings.json
```

#### パラメーター
| 名称                  | field    | 備考 |
| :--                   | :--      | :--|
| ページ番号            | page     | |
| 1ページあたりの表示数 | per_page | 100件まで |

#### リクエスト例
```
curl -i -H "Authorization: BEARER [ACCESS_TOKEN]" "https://invoice.moneyforward.com/api/v1/billings.json?page=1&per_page=10"
```

#### レスポンス
```
HTTP/1.1 200 OK

{
  "meta" : {
    "total_count" : 1,
    "total_pages" : 1,
    "current_page" : 1,
    "per_page" : 10
  },
  "billings" : [
    {
      "id" : "ABCDEFGHIJKLMNOPQRST123",
      "user_id" : "ABCDEFGHIJKLMNOPQRST456",
      "partner_id" : "ABCDEFGHIJKLMNOPQRST789",
      "department_id" : "ABCDEFGHIJKLMNOPQRST012",
      "partner_name" : "サンプル取引先",
      "partner_name_suffix" : "様",
      "partner_detail" : "hogehoge",
      "member_id" : "ABCDEFGHIJKLMNOPQRST345",
      "member_name" : "担当者A",
      "office_name" : "サンプル事業所",
      "office_detail" : "",
      "title" : "件名サンプル",
      "excise_price" : 80,
      "deduct_price" : 0,
      "subtotal" : 1000,
      "memo" : "",
      "payment_condition" : "",
      "total_price" : 1080,
      "billing_date" : "2015/10/31",
      "due_date" : "2015/11/30",
      "sales_date" : "2015/10/31",
      "created_at" : "2015/10/31T00:00:00.000+09:00",
      "updated_at" : "2015/10/31T00:00:00.000+09:00",
      "billing_number": "1",
      "note": "",
      "document_name": "",
      "tags": [
        "tag1",
        "tag2"
      ],
      "status" : {
        "posting" : "未郵送",
        "email" : "未送信",
        "download" : "",
        "payment" : "未設定"
      },
      "items" : [
        {
          "id" : "ABCDEFGHIJKLMNOPQRST012",
          "code" : "ITEM-001",
          "name" : "商品A",
          "detail" : "",
          "quantity" : 1,
          "unit_price" : 1000,
          "unit" : "個",
          "price" : 1000,
          "display_order" : 0,
          "excise": true,
          "created_at" : "2015/10/31T00:00:00.000+09:00",
          "updated_at" : "2015/10/31T00:00:00.000+09:00"
        }
      ]
    }
  ]
}
```

### 請求書一覧の検索
```
  GET /api/v1/billings/search.json
```

#### パラメーター
| 名称                  | field     | 備考 |
| :--                   | :--       | :-- |
| ページ番号            | page      | |
| 1ページあたりの表示数 | per_page  | 100件まで |
| 検索文字列            | q         |  取引先(完全一致), ステータス、件名etc |
| 期間絞込対象          | range_key |  created_at(デフォルト), sales_date, billing_dateのみ許可 |
| 期間開始日            | from      |  デフォルト: 月初 |
| 期間終了日            | to        |  デフォルト: 月末 |

#### リクエスト例
```
curl -i -H "Authorization: BEARER [ACCESS_TOKEN]" "https://invoice.moneyforward.com/api/v1/billings/search.json?q=hoge&range_key=created_at&from=2015-10-01&to=2015-10-31"
```

#### レスポンス
```
HTTP/1.1 200 OK

{
  "meta" : {
    "total_count" : 1,
    "total_pages" : 1,
    "current_page" : 1,
    "per_page" : 100,
    "condition" : {
      "query" : "hoge",
      "range_key" : "created_at",
      "from" : "2015-10-01",
      "to" : "2015-10-31"
    }
  },
  "billings" : [
    {
      "id" : "ABCDEFGHIJKLMNOPQRST123",
      "user_id" : "ABCDEFGHIJKLMNOPQRST456",
      "partner_id" : "ABCDEFGHIJKLMNOPQRST789",
      "department_id" : "ABCDEFGHIJKLMNOPQRST012",
      "partner_name" : "サンプル取引先",
      "partner_name_suffix" : "様",
      "partner_detail" : "hogehoge",
      "member_id" : "ABCDEFGHIJKLMNOPQRST345",
      "member_name" : "担当者A",
      "office_name" : "サンプル事業所",
      "office_detail" : "",
      "title" : "件名サンプル",
      "excise_price" : 80,
      "deduct_price" : 0,
      "subtotal" : 1000,
      "memo" : "",
      "payment_condition" : "",
      "total_price" : 1080,
      "payment_condition" : "",
      "billing_date" : "2015/10/31",
      "due_date" : "2015/11/30",
      "sales_date" : "2015/10/31",
      "created_at" : "2015/10/31T00:00:00.000+09:00",
      "updated_at" : "2015/10/31T00:00:00.000+09:00",
      "billing_number" : "1",
      "note" : "",
      "document_name" : "",
      "tags": [
        "tag1",
        "tag2"
      ],
      "status" : {
        "posting" : "未郵送",
        "email" : "未送信",
        "download" : "",
        "payment" : "未設定"
      },
      "items" : [
        {
          "id" : "ABCDEFGHIJKLMNOPQRST012",
          "code" : "ITEM-001",
          "name" : "商品A",
          "detail" : "",
          "quantity" : 1,
          "unit_price" : 1000,
          "unit" : "個",
          "price" : 1000,
          "excise": true,
          "display_order" : 0,
          "created_at" : "2015/10/31T00:00:00.000+09:00",
          "updated_at" : "2015/10/31T00:00:00.000+09:00"
        }
      ]
    }
  ]
}
```

### 請求書の取得
```
  GET /api/v1/billings/:id.json
```

#### パラメーター
なし

#### リクエスト例
```
curl -i -H "Authorization: BEARER [ACCESS_TOKEN]" https://invoice.moneyforward.com/api/v1/billings/ABCDEFGHIJKLMNOPQRST123.json
```

#### レスポンス
```
HTTP/1.1 200 OK

{
  "id" : "ABCDEFGHIJKLMNOPQRST123",
  "pdf_url" : "https://invoice.moneyforward.co.jp/api/v1/billings/:id.pdf",
  "user_id" : "ABCDEFGHIJKLMNOPQRST456",
  "partner_id" : "ABCDEFGHIJKLMNOPQRST789",
  "department_id" : "ABCDEFGHIJKLMNOPQRST012",
  "partner_name" : "サンプル取引先",
  "partner_name_suffix" : "様",
  "partner_detail" : "hogehoge",
  "member_id" : "ABCDEFGHIJKLMNOPQRST345",
  "member_name" : "担当者A",
  "office_name" : "サンプル事業所",
  "office_detail" : "",
  "title" : "件名サンプル",
  "excise_price" : 80,
  "deduct_price" : 0,
  "subtotal" : 1000,
  "memo" : "",
  "payment_condition" : "",
  "total_price" : 1080,
  "payment_condition" : "",
  "billing_date" : "2015/10/31",
  "due_date" : "2015/11/30",
  "sales_date" : "2015/10/31",
  "created_at" : "2015/10/31T00:00:00.000+09:00",
  "updated_at" : "2015/10/31T00:00:00.000+09:00",
  "billing_number" : "1",
  "note" : "",
  "document_name" : "",
  "tags": [
    "tag1",
    "tag2"
  ],
  "status" : {
    "posting" : "未郵送",
    "email" : "未送信",
    "download" : "",
    "payment" : "未設定"
  },
  "items" : [
    {
      "id" : "ABCDEFGHIJKLMNOPQRST012",
      "name" : "商品A",
      "code" : "ITEM-001",
      "detail" : "",
      "quantity" : 1,
      "unit_price" : 1000,
      "unit" : "個",
      "price" : 1000,
      "excise": true,
      "display_order" : 0,
      "created_at" : "2015/10/31T00:00:00.000+09:00",
      "updated_at" : "2015/10/31T00:00:00.000+09:00"
    }
  ]
}
```

#### エラーレスポンス
```
{
  "code" : "404",
  "errors" : [
    {
      "message" : "存在しないIDが渡されました。"
    }
  ]
}
```

### 請求書pdfの取得
```
  GET /api/v1/billings/:id.pdf
```

#### パラメーター
なし

#### リクエスト例
```
curl -i -H "Authorization: BEARER [ACCESS_TOKEN]" https://invoice.moneyforward.com/api/v1/billings/ABCDEFGHIJKLMNOPQRST123.pdf -O
```

#### レスポンス
pdfファイルがバイナリで送信されます。
```
HTTP/1.1 200 OK
Content-Type: application/pdf
```

#### エラーレスポンス
```
HTTP/1.1 404 Not Found

{
  "code" : "404",
  "errors" : [
    {
      "message" : "存在しないIDが渡されました。"
    }
  ]
}
```

### 請求書の作成
```
  POST /api/v1/billings
```

#### パラメーター
| 名称         | field                      | 備考 |
| :--          | :--                        | :-- |
| 部門ID       | billing[department_id]     | 必須 |
| 件名         | billing[title]             | |
| 請求書番号   | billing[billing_number]    | |
| 振込先       | billing[payment_condition] | |
| 備考         | billing[note]              | |
| 請求日       | billing[billing_date]      | |
| お支払期限   | billing[due_date]          | |
| 売上計上日   | billing[sales_date]        | |
| メモ         | billing[memo]              | |
| 帳票名       | billing[document_name]     | |
| タグ         | billing[tags]              | カンマ区切り文字列で記載 |
| 品目1 名前   | items[0][name]             | |
| 品目1 コード | items[0][code]             | |
| 品目1 詳細   | items[0][detail]           | |
| 品目1 数量   | items[0][quantity]         | |
| 品目1 単価   | items[0][unit_price]       | |
| 品目1 単位   | items[0][unit]             | |
| 品目1 税対象 | items[0][excise]           | 0: false  1: true |
| 品目2 名前   | items[1][name]             | |
| 品目2 コード | items[1][code]             | |
| 品目2 詳細   | items[1][detail]           | |
| 品目2 数量   | items[1][quantity]         | |
| 品目2 単価   | items[1][unit_price]       | |
| 品目2 単位   | items[1][unit]             | |
| 品目2 税対象 | items[1][excise]           | 0: false  1: true |

#### リクエスト例
```
curl -i -H "Authorization: BEARER [ACCESS_TOKEN]" -H "Content-Type: application/json" -d '{ "billing" : { "department_id" : "DEPARTMENT_ID" }}' -X POST https://invoice.moneyforward.com/api/v1/billings

# 品目を含むリクエスト
curl -i -H "Authorization: BEARER [ACCESS_TOKEN]" -H "Content-Type: application/json" \
-d '
{
  "billing" : {
    "department_id" : "DEPARTMENT_ID",
      "items" : [
        {
          "name" : "商品A",
          "quantity" : "1",
          "unit_price" : "100"
        },
        # 登録した品目を利用する場合
        {
          "code" : "登録済み品目コード"
        }
      ]
  }
} '
-X POST https://invoice.moneyforward.com/api/v1/billings

# タグを含むリクエスト
curl -i -H "Authorization: BEARER [ACCESS_TOKEN]" -H "Content-Type: application/json" \
-d '
{
  "billing" : {
    "department_id" : "DEPARTMENT_ID",
      "items" : [
        {
          "name" : "商品A",
          "quantity" : "1",
          "unit_price" : "100"
        }
      ],
    "tags" : "tag1,tag2" # カンマ区切り文字列で入力
  }
} '
-X POST https://invoice.moneyforward.com/api/v1/billings
```
#### レスポンス
```
HTTP/1.1 201 Created

{
  "id" : "ABCDEFGHIJKLMNOPQRST123",
  "pdf_url" : "https://invoice.moneyforward.co.jp/api/v1/billings/:id.pdf",
  "user_id" : "ABCDEFGHIJKLMNOPQRST456",
  "partner_id" : "ABCDEFGHIJKLMNOPQRST789",
  "department_id" : "ABCDEFGHIJKLMNOPQRST012",
  "partner_name" : "サンプル取引先",
  "partner_name_suffix" : "様",
  "partner_detail" : "hogehoge",
  "member_id" : "ABCDEFGHIJKLMNOPQRST345",
  "member_name" : "担当者A",
  "office_name" : "サンプル事業所",
  "office_detail" : "",
  "title" : "件名サンプル",
  "excise_price" : 80,
  "deduct_price" : 0,
  "subtotal" : 1000,
  "memo" : "",
  "payment_condition" : "",
  "total_price" : 1080,
  "billing_date" : "2015/10/31",
  "due_date" : "2015/11/30",
  "sales_date" : "2015/10/31",
  "created_at" : "2015/10/31T00:00:00.000+09:00",
  "updated_at" : "2015/10/31T00:00:00.000+09:00",
  "billing_number" : "1",
  "note" : "",
  "document_name" : "",
  "tags": [
    "tag1",
    "tag2"
  ],
  "status" : {
    "posting" : "未郵送",
    "email" : "未送信",
    "download" : "",
    "payment" : "未設定"
  },
  "items" : [
    {
      "id" : "ABCDEFGHIJKLMNOPQRST012",
      "name" : "商品A",
      "detail" : "",
      "quantity" : 1,
      "unit_price" : 1000,
      "unit" : "個",
      "price" : 1000
      "excise": true,
      "display_order" : 0,
      "created_at" : "2015/10/31T00:00:00.000+09:00",
      "updated_at" : "2015/10/31T00:00:00.000+09:00"
    }
  ]
}
```

#### エラーレスポンス
department_idが不正であった場合
```
{
  "code" : "404",
  "errors" : [
    {
      "message" : "存在しないIDが渡されました。"
    }
  ]
}
```

### 請求書の更新
```
  PATCH /api/v1/billings/:id
```
#### パラメーター
| 名称         | field                      | 備考 |
| :--          | :--                        | :-- |
| 部門ID       | billing[department_id]     | |
| 件名         | billing[title]             | |
| 請求書番号   | billing[billing_number]    | |
| 振込先       | billing[payment_condition] | |
| 備考         | billing[note]              | |
| 請求日       | billing[billing_date]      | |
| お支払期限   | billing[due_date]          | |
| 売上計上日   | billing[sales_date]        | |
| メモ         | billing[memo]              | |
| 帳票名       | billing[document_name]     | |
| タグ         | billing[tags]              | カンマ区切り文字列で記載 |
| 品目1 名前   | items[0][name]             | |
| 品目1 コード | items[0][code]             | |
| 品目1 詳細   | items[0][detail]           | |
| 品目1 数量   | items[0][quantity]         | |
| 品目1 単価   | items[0][unit_price]       | |
| 品目1 単位   | items[0][unit]             | |
| 品目1 税対象 | items[0][excise]           | 0: false  1: true |
| 品目2 名前   | items[1][name]             | |
| 品目2 コード | items[1][code]             | |
| 品目2 詳細   | items[1][detail]           | |
| 品目2 数量   | items[1][quantity]         | |
| 品目2 単価   | items[1][unit_price]       | |
| 品目2 単位   | items[1][unit]             | |
| 品目2 税対象 | items[1][excise]           | 0: false  1: true |

#### リクエスト例
```
curl -i -H "Authorization: BEARER [ACCESS_TOKEN]" -H "Content-Type: application/json" -d '{ "billing" : { "title" : "件名サンプル" }}' -X PATCH https://invoice.moneyforward.com/api/v1/billings/ABCDEFGHIJKLMNOPQRST123

# 品目を含むリクエスト
curl -i -H "Authorization: BEARER [ACCESS_TOKEN]" -H "Content-Type: application/json" \
-d '
{
  "billing" : {
    "department_id" : "DEPARTMENT_ID",
      "items" : [
        # itemの追加(idなし)
        {
          "name" : "商品C",
          "quantity" : "1",
          "unit_price" : "100"
        },
        # itemの更新(idあり)
        {
          "id" : "ABCDEFGHIJKLMNOPQRST012"
          "name" : "商品B",
          "quantity" : "1",
          "unit_price" : "100"
        },
        # itemの更新(削除)
        {
          "id" : "ABCDEFGHIJKLMNOPQRST012"
          "_destroy" : true
        },
      ]
  }
} '
-X PATCH https://invoice.moneyforward.com/api/v1/billings/"更新する請求書ID"
```

#### レスポンス
```
HTTP/1.1 200 OK

{
  "id" : "ABCDEFGHIJKLMNOPQRST123",
  "pdf_url" : "https://invoice.moneyforward.co.jp/api/v1/billings/:id.pdf",
  "user_id" : "ABCDEFGHIJKLMNOPQRST456",
  "partner_id" : "ABCDEFGHIJKLMNOPQRST789",
  "department_id" : "ABCDEFGHIJKLMNOPQRST012",
  "partner_name" : "サンプル取引先",
  "partner_name_suffix" : "様",
  "partner_detail" : "hogehoge",
  "member_id" : "ABCDEFGHIJKLMNOPQRST345",
  "member_name" : "担当者A",
  "office_name" : "サンプル事業所",
  "office_detail" : "",
  "title" : "件名サンプル",
  "excise_price" : 80,
  "deduct_price" : 0,
  "subtotal" : 1000,
  "memo" : "",
  "payment_condition" : "",
  "total_price" : 1080,
  "billing_date" : "2015/10/31",
  "due_date" : "2015/11/30",
  "sales_date" : "2015/10/31",
  "created_at" : "2015/10/31T00:00:00.000+09:00",
  "updated_at" : "2015/10/31T00:00:00.000+09:00",
  "billing_number" : "1",
  "note" : "",
  "document_name" : "",
  "tags": [
    "tag1",
    "tag2"
  ],
  "status" : {
    "posting" : "未郵送",
    "email" : "未送信",
    "download" : "",
    "payment" : "未設定"
  },
  "items" : [
    {
      "id" : "ABCDEFGHIJKLMNOPQRST012",
      "code" : "ITEM-001",
      "name" : "商品A",
      "detail" : "",
      "quantity" : 1,
      "unit_price" : 1000,
      "unit" : "個",
      "price" : 1000,
      "excise": true,
      "display_order" : 0,
      "created_at" : "2015/10/31T00:00:00.000+09:00",
      "updated_at" : "2015/10/31T00:00:00.000+09:00"
    }
  ]
}
```

#### エラーレスポンス
IDが不正である場合
```
HTTP/1.1 404 Not Found

{
  "code" : "404",
  "errors" : [
    {
      "message" : "存在しないIDが渡されました。"
    }
  ]
}
```

### 請求書の郵送
```
  POST /api/v1/billings/:id/posting
```

#### パラメーター
なし

#### リクエスト例
```
curl -i -H "Authorization: BEARER [ACCESS_TOKEN]" -H "Content-Type: application/json" -d '' -X POST https://invoice.moneyforward.com/api/v1/billings/ABCDEFGHIJKLMNOPQRST123/posting
```

#### レスポンス
```
HTTP/1.1 200 OK
```

#### エラーレスポンス
既に郵送済みである場合
```
HTTP/1.1 400 Bad Request

{
  "code" : "400",
  "errors" : [
    {
      "message" : "既に郵送済みです"
    }
  ]
}
```

### 請求書の郵送キャンセル
```
  POST /api/v1/billings/:id/cancel_posting
```

#### パラメーター
なし

#### リクエスト例
```
curl -i -H "Authorization: BEARER [ACCESS_TOKEN]" -H "Content-Type: application/json" -d '' -X POST https://invoice.moneyforward.com/api/v1/billings/ABCDEFGHIJKLMNOPQRST123/cancel_posting
```

#### レスポンス
```
HTTP/1.1 200 OK
```

### 請求書の削除
```
  DELETE /api/v1/billings/:id
```

#### パラメーター
なし

#### リクエスト例
```
curl -i -H "Authorization: BEARER [ACCESS_TOKEN]" -X DELETE https://invoice.moneyforward.com/api/v1/billings/ABCDEFGHIJKLMNOPQRST123
```

#### レスポンス
```
HTTP/1.1 204 No Content
```

#### エラーレスポンス
IDが不正である場合
```
HTTP/1.1 404 Not Found

{
  "code" : "404",
  "errors" : [
    {
      "message" : "存在しないIDが渡されました。"
    }
  ]
}
```

## 品目API
### エンドポイント
```
  /api/v1/items
```

### 品目一覧の取得
```
  GET /api/v1/items.json
```

#### パラメーター
| 名称                  | field    | 備考 |
| :--                   | :--      | :--|
| ページ数              | page     | |
| 1ページあたりの表示数 | per_page | 100件まで |

#### リクエスト例
```
curl -i -H "Authorization: BEARER [ACCESS_TOKEN]" "https://invoice.moneyforward.com/api/v1/items.json?page=1&per_page=10"
```

#### レスポンス
HTTP/1.1 200 OK

```
{
  "meta" : {
    "total_count" : 1,
    "total_pages" : 1,
    "current_page" : 1,
    "per_page" : 10
  },
  "items": [
    {
      "id" : "ABCDEFGHIJKLMNOPQRST123",
      "code" : "ITEM-001",
      "name" : "サンプル商品",
      "detail" : "",
      "unit_price" : 1000,
      "unit" : "個",
      "quantity" : 1,
      "price" : 1000,
      "excise" : true,
      "created_at" : "2015/10/31T00:00:00.000+09:00",
      "updated_at" : "2015/10/31T00:00:00.000+09:00"
    }
  ]
}
```

### 品目の取得
```
  GET /api/v1/items/:id.json
```

#### パラメーター
なし

#### リクエスト例
```
curl -i -H "Authorization: BEARER [ACCESS_TOKEN]" https://invoice.moneyforward.com/api/v1/items/ABCDEFGHIJKLMNOPQRST123.json
```

#### レスポンス
HTTP/1.1 200 OK

```
{
  "id" : "ABCDEFGHIJKLMNOPQRST123",
  "code" : "ITEM-001",
  "name" : "サンプル商品",
  "detail" : "",
  "unit_price" : 1000,
  "unit" : "個",
  "quantity" : 1,
  "price" : 1000,
  "excise" : true,
  "created_at" : "2015/10/31T00:00:00.000+09:00",
  "updated_at" : "2015/10/31T00:00:00.000+09:00"
}
```

### 品目の作成
```
  POST /api/v1/items
```

#### パラメーター
| 名称               | field            | 備考 |
| :--                | :--              | :--|
| 名前               | item[name]       | 必須 |
| 品目コード         | item[code]       | |
| 詳細               | item[detail]     | |
| 単価               | item[unit_price] | |
| 単位               | item[unit]       | |
| 数量               | item[quantity]   | |
| 消費税を計算するか | item[excise]     | 0: false  1: true |

#### リクエスト例
```
curl -i -H "Authorization: BEARER [ACCESS_TOKEN]" -H "Content-Type: application/json" -d '{ "item" : { "name" : "サンプル商品" }}' -X POST https://invoice.moneyforward.com/api/v1/items
```

#### レスポンス
```
HTTP/1.1 201 Created

{
  "id" : "ABCDEFGHIJKLMNOPQRST123",
  "name" : "サンプル商品",
  "code" : "ITEM-001",
  "detail" : "",
  "unit_price" : 1000,
  "unit" : "個",
  "quantity" : 1,
  "price" : 1000,
  "excise" : true,
  "created_at" : "2015/10/31T00:00:00.000+09:00",
  "updated_at" : "2015/10/31T00:00:00.000+09:00"
}
```

#### エラーレスポンス
```
{
  "code" : "402",
  "errors" : [
    {
      "message" : "名前を入力してください"
    }
  ]
}
```

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
```
HTTP/1.1 200 OK

{
  "id" : "ABCDEFGHIJKLMNOPQRST123",
  "name" : "サンプル商品2",
  "code" : "ITEM-001",
  "detail" : "",
  "unit_price" : 1000,
  "unit" : "個",
  "quantity" : 1,
  "price" : 1000,
  "excise" : true,
  "created_at" : "2015/10/31T00:00:00.000+09:00",
  "updated_at" : "2015/10/31T00:00:00.000+09:00"
}
```

#### エラーレスポンス
IDが不正である場合
```
HTTP/1.1 404 Not Found

{
  "code" : "404",
  "errors" : [
    {
      "message" : "存在しないIDが渡されました。"
    }
  ]
}
```

### 品目の削除
```
  DELETE /api/v1/items/:id
```

#### パラメーター
なし

#### リクエスト例
```
curl -i -H "Authorization: BEARER [ACCESS_TOKEN]" -X DELETE https://invoice.moneyforward.com/api/v1/items/ABCDEFGHIJKLMNOPQRST123
```

#### レスポンス
```
HTTP/1.1 204 No Content
```

#### エラーレスポンス
IDが不正である場合
```
HTTP/1.1 404 Not Found

{
  "code" : "404",
  "errors" : [
    {
      "message" : "存在しないIDが渡されました。"
    }
  ]
}
```

## 送付履歴API
### エンドポイント
```
  /api/v1/sent_history
```

### 送付履歴一覧の取得
```
  GET /api/v1/sent_history.json
```

#### パラメーター
| 名称                  | field    | 備考 |
| :--                   | :--      | :--|
| ページ数              | page     | |
| 1ページあたりの表示数 | per_page | 100件まで |

#### リクエスト例
```
curl -i -H "Authorization: BEARER [ACCESS_TOKEN]" "https://invoice.moneyforward.com/api/v1/sent_history.json?page=1&per_page=10"
```

#### レスポンス
HTTP/1.1 200 OK

```
{
  "meta": {
    "total_count" : 1,
    "total_pages" : 1,
    "current_page" : 1,
    "per_page" : 10
  },
  "sent_history_list": [
    {
      "operator_id":  "ABCDEFGHIJKLMNOP",
      "type": "メール",
      "document_type": "請求書",
      "document_id": "ABCDEFGHIJKLMNOP",
      "from": "",
      "to": "sample@moneyforward.co.jp",
      "cc": "",
      "sent_at": "2015-05-15T11:40:44.000+09:00"
    }
  ]
}
```
