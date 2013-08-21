# paperboy-all.github.io

paperboy&amp;co.の公開ドキュメントを置くためのリポジトリです。以下のURLでサーブされています。

[http://paperboy-all.github.io/](http://paperboy-all.github.io/)

## ブランチ

  * 管理用: manageブランチ(default branchに指定されています)
  * 公開用: masterブランチ

## 書き方

[middleman](http://middleman-guides.e2esound.com/)を使ってます。

  1. `source/docs`以下に、必要ならば適当にディレクトリをほる
  2. `.md`という拡張子で、markdownで書く
  3. `$ bundle exec middleman`でサーバを起動し、[http://localhost:4567/](http://localhost:4567/)にアクセス
  4. manageブランチに対してpull requestを送る

## デプロイ

`middleman deploy`コマンドを使います。

```
$ bundle exec middleman deploy
```

上記で、このリポジトリのmasterブランチにpushされ、それが[http://paperboy-all.github.io/](http://paperboy-all.github.io/)に反映されます。
