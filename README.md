# MFクラウド請求書APIドキュメント
## versions

|version|link|
|:--:|:--:|
|v1|https://github.com/nishisuke/invoice-api-doc/tree/v1|

## Resources
- [見積書](/quotes)
- [事業所](/office)
- [取引先](/partners)
- [品目](/items)
- [請求書](/billings)

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

### アクセストークンの再発行

発行されたアクセストークンの有効期限は、30日間です。
期限切れアクセストークンの再発行については[MFクラウド請求書API スタートアップガイド](https://support.biz.moneyforward.com/invoice/guide/api-guide/a01.html)に記載していますのでそちらをご覧ください。

## アクセス数の制限について
* Basicプラン以下のプランをご利用の場合、月のAPI経由での請求書作成数を100件までとさせていただきます。
* その他、取引先登録数や郵送の可否等は通常のプランごとの制限と同様* となります。

## クライアントライブラリ

* [Ruby](https://github.com/moneyforward/mf_cloud-invoice-ruby)

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
