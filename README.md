# マネーフォワード クラウド請求書APIドキュメント
このページは **v2** のドキュメントです。

## バージョン

|version|link|
|:--:|:--:|
|v1|https://github.com/moneyforward/invoice-api-doc/tree/v1|
|v2|[here](https://github.com/moneyforward/invoice-api-doc/)|

## format
v2のレスポンスは[jsonapi](http://jsonapi.org/format/)に従っています。

## リソース
- [事業所](/office)
- [取引先](/partners)
- [品目](/items)
- [請求書](/billings)
- [送付履歴](/sent_histories)
- [見積書](/quotes)

＊ *見積書はv2から利用可能になりました。*

## プランごとの利用制限について
どのプランでも下記制限の中でAPIを利用することができます。

| プラン     | 各帳票作成リクエスト上限 |
| :--        | --:                      |
| Free       | 100                      |
| Basic      | 100                      |
| Pro        | なし                     |

* その他、取引先登録数や郵送の可否等は通常のプランごとの制限と同様* となります。

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

## クライアントライブラリ

* [Ruby](https://github.com/moneyforward/mf_cloud-invoice-ruby)
