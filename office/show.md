### エンドポイント
```
  /api/v2/office
```
### 事業所情報の取得

```
  GET /api/v2/office.json
```

#### パラメーター
なし

### リクエスト例
```
curl -i -H "Authorization: BEARER [ACCESS_TOKEN]" https://invoice.moneyforward.com/api/v2/office.json
```

#### レスポンス

HTTP/1.1 200 OK

[office.json](./responses/office.json)
