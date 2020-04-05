# 概要

Dockerについてのメモ

# alpineについて

基本的には `apk` を使って各種ライブラリなどをインストールする

その際、
- autoconf
- gcc
- g++
- make

などをインストールしないとビルドできないものがあるので、インストールして、最終的にdelするのが軽量化に繋がる。

```
# build apps
RUN apk update \
    && apk add --no-cache --virtual .buildapps autoconf gcc g++ make

# delete build apps
RUN apk del .buildapps
```

`apk` の `--virtual` オプションはインストールしたものに名前をつけて、あとからまとめて操作が可能になる

# dive

dockerのlayerを確認できるツール

https://github.com/wagoodman/dive

