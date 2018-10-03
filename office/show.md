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
