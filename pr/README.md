# pr

pull request を操作してみる

## 作成

```bash
curl -X POST \
    -H "Accept: application/vnd.github.v3+json" \
    -u ${GITHUB_USER}:${GITHUB_PERSONAL_TOKEN} \
    https://api.github.com/repos/pa-k-sato/github-rest-api/pulls \
    -d '{"head":"feature1","base":"main","title":"test pr title","body":"test pr body","draft":true}'
```

## 編集

タグとかつけたい。あと本文の改行も試してみる

```bash
# 本文更新
curl -X PATCH \
    -H "Accept: application/vnd.github.v3+json" \
    -u ${GITHUB_USER}:${GITHUB_PERSONAL_TOKEN} \
    https://api.github.com/repos/pa-k-sato/github-rest-api/pulls/1 \
    -d '{"body":"test pr body \n line feeded?"}'

# アサインする
curl -X POST \
    -H "Accept: application/vnd.github.v3+json" \
    -u ${GITHUB_USER}:${GITHUB_PERSONAL_TOKEN} \
    https://api.github.com/repos/pa-k-sato/github-rest-api/issues/1/assignees \
    -d '{"assignees": ["pa-k-sato"]}'

# ラベルをつける issue エンドポイントでやるっぽい
curl -X POST \
    -H "Accept: application/vnd.github.v3+json" \
    -u ${GITHUB_USER}:${GITHUB_PERSONAL_TOKEN} \
    https://api.github.com/repos/pa-k-sato/github-rest-api/issues/1/labels \
    -d '{"labels": ["enhancement","documentation"]}'

# プロジェクトに追加する
# 先にプロジェクトにカラムを作っておく必要があり、そのカラムに （PRと紐付ける） カードを追加する
# 事前情報の取得が結構面倒
# 17946021 は対象プロジェクトの追加先のカラム
# 871324725 は PR の ID。URL で見えている番号とは違うので別途取得しなきゃいけない
#   POST した時のレスポンスを保持しておくの良いのかもしれない
curl -X POST \
    -H "Accept: application/vnd.github.v3+json" \
    -u ${GITHUB_USER}:${GITHUB_PERSONAL_TOKEN} \
    https://api.github.com/projects/columns/17946021/cards \
    -d '{"content_id": 871324725, "content_type": "PullRequest"}'

# マイルストーンをつける
curl -X PATCH \
    -H "Accept: application/vnd.github.v3+json" \
    -u ${GITHUB_USER}:${GITHUB_PERSONAL_TOKEN} \
    https://api.github.com/repos/pa-k-sato/github-rest-api/issues/1 \
    -d '{"milestone": 1}'

# コメントを追加
curl -X POST \
    -H "Accept: application/vnd.github.v3+json" \
    -u ${GITHUB_USER}:${GITHUB_PERSONAL_TOKEN} \
    https://api.github.com/repos/pa-k-sato/github-rest-api/issues/1/comments \
    -d '{"body": "rest api からコメント @pa-k-sato"}'
```

以下はこのリポジトリでは試せてない

```bash
# reviewer を追加
curl -X POST \
    -H "Accept: application/vnd.github.v3+json" \
    -u ${GITHUB_USER}:${GITHUB_PERSONAL_TOKEN} \
    https://api.github.com/repos/pa-k-sato/github-rest-api/pulls/1/requested_reviewers \
    -d '{"reviewers": ["user_name-hoge","user_name-fuga"]}'
```

## 参考

前提となる作業など

```bash
# pull request
curl \
    -H "Accept: application/vnd.github.v3+json" \
    -u ${GITHUB_USER}:${GITHUB_PERSONAL_TOKEN} \
    https://api.github.com/repos/pa-k-sato/github-rest-api/pulls/1

# リポジトリのプロジェクト一覧、ここでプロジェクト ID を取る必要がある
curl \
    -H "Accept: application/vnd.github.v3+json" \
    -u ${GITHUB_USER}:${GITHUB_PERSONAL_TOKEN} \
    https://api.github.com/repos/pa-k-sato/github-rest-api/projects

# プロジェクトのカラム一覧
curl \
    -H "Accept: application/vnd.github.v3+json" \
    -u ${GITHUB_USER}:${GITHUB_PERSONAL_TOKEN} \
    https://api.github.com/projects/14195572/columns

# リポジトリのマイルストーン一覧、ここで ID を取る必要がある
curl \
    -H "Accept: application/vnd.github.v3+json" \
    -u ${GITHUB_USER}:${GITHUB_PERSONAL_TOKEN} \
    https://api.github.com/repos/pa-k-sato/github-rest-api/milestones

curl \
    -H "Accept: application/vnd.github.v3+json" \
    -u ${GITHUB_USER}:${GITHUB_PERSONAL_TOKEN} \
    https://api.github.com/repos/pa-k-sato/github-rest-api/milestones/1
```

