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

HTTP/1.1 200 OK

[office.json](./responses/office.json)

#### エラーレスポンス
アクセストークンが不正な場合
```
HTTP/1.1 401 Unauthorized
```
