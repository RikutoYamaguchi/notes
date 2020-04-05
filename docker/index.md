# 概要

Dockerについてのメモ

# Docker BuildKit

**Dockerデーモン設定**

docker desktopの `Preference` → `Docker Engine` で

```json
  "features": {
    "buildkit": true
  }
```

追加して、 `Apply & Restart`

# alpineについて

軽量化Linuxディストリビューション

基本的には `apk` を使って各種ライブラリなどをインストールする

その際、
- autoconf
- gcc
- g++
- make

などをインストールしないとビルドできないものがあるので、インストールして、最終的にdelするのが軽量化に繋がる。

```dockerfile
# build apps
RUN apk update \
    && apk add --no-cache --virtual .buildapps autoconf gcc g++ make

# その他のライブラリなど

# delete build apps
RUN apk del .buildapps
```

`apk` の `--virtual` オプションはインストールしたものに名前をつけて、あとからまとめて操作が可能になる

※RUNを多用するとレイヤーがたくさんんできるっぽいので、一個にまとめたほうがよい？ TODO: もうちょっと調査する

# dive

dockerのlayerを確認できるツール

https://github.com/wagoodman/dive

# マルチステージビルドについて

TODO: 使い方実践する

```dockerfile
FROM apline as DEV
RUN echo 'hello world! > hello.txt

FROM apline as PROD
COPY --from=DEV /hello.txt .
CMD ["cat", "/hello.txt"]
```

みたいにして書くことができる

`docker build --target <ASでつけた名前>` でビルドできるけど、実践的にどう使うかまだ分からない

# 参考情報

https://www.slideshare.net/zembutsu/explaining-best-practices-for-writing-dockerfiles

https://www.slideshare.net/zembutsu/dockerfile-bestpractices-19-and-advice

