# pr

pull request を操作してみる

## 作成

```bash
url -X POST \
    -H "Accept: application/vnd.github.v3+json" \
    -u ${GITHUB_USER}:${GITHUB_PERSONAL_TOKEN} \
    https://api.github.com/repos/pa-k-sato/github-rest-api/pulls \
    -d '{"head":"feature1","base":"main","title":"test pr title","body":"test pr body","draft":true}'
```
