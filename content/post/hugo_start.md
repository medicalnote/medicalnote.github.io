---
title: "Hugo開始メモ"
date: 2017-08-15T17:09:44+09:00
tags: ["hugo","サイト生成"]
author: "sakurai"
slug : ""
draft: false
---

- インストール
```
brew install hugo
```

- 新規作成
```
hugo new site *****
```

- ローカル起動 + テンプレートテーマ適用(新規作成したhugoのプロジェクトrootで)
```
hugo server -t minimal -D -w
```

- 記事作成
```
hugo new post/*****.md
```

- 参考URL<br>
[HUGOで静的ページサイトを構築する (1/3)](https://holywise.github.io/2016/12/how_to_setup_hugo_site/)<br>
[Shortcode](http://qiita.com/waytoa/items/4107945cbd3883f51755)<br>
[【Hugo】ゴリラでも分かる静的サイトジェネレーターHugoでオリジナルテーマ作り](https://blog.mismithportfolio.com/web/20160207hugomyblog)<br>
[Hugoでwebサイト構築(4) markdownで記事を書く](http://wdkk.co.jp/note/2015/0817-hugo-markdown/)<br>
[Hugoを使ってAmazon S3でウェブサイトを構築してみた](http://dev.classmethod.jp/cloud/hugo-s3/)<br>
[HugoとGitHub Pagesで静的サイトを公開する](http://qiita.com/satzz/items/e24bd703fc04fb45f7ef)<br>
[HugoとCircleCIでGitHub PagesにBlogを公開してみた](https://hori-ryota.com/blog/create-blog-with-hugo-and-circleci/)<br>
[Hugoの執筆者を複数人で切り分ける方法](http://musette.xyz/2017/02/how-to-make-multiple-author-on-hugo/)<br>






















