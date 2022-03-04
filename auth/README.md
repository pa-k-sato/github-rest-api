# auth

認証周り

## ユーザをどうやって特定するか

認証なしで

```bash
curl https://api.github.com/user
```

を実行するとレスポンスが下記のエラーとなる  
これはどのユーザかを特定できない or 権限が必要なエンドポイントに匿名でアクセスしているから  
と思われる（ and かもしれない）

```json
{
  "message": "Requires authentication",
  "documentation_url": "https://docs.github.com/rest/reference/users#get-the-authenticated-user"
}
```

で、個人トークンを利用するとちゃんと自分の情報が返ってくるようになる

```bash
# 事前に情報設定
export GITHUB_USER=<your_username>
export GITHUB_PERSONAL_TOKEN=<your_personal_token>

curl -u ${GITHUB_USER}:${GITHUB_PERSONAL_TOKEN} https://api.github.com/user
```

