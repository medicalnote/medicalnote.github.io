---
title: "Push通知サービス検討メモ"
date: 2017-08-15T16:27:03+09:00
tags: ["mobile","Push通知","iOS","Android"]
author: "takada"
slug : ""
draft: false
---

iOS（ゆくゆくはAndroid）へのPush通知を実装する上で
どういうサービスを使うべきかということに関しての検討まとめメモ

## 今回の調査における前提

ざっくり調査するうえで以下の観点で見ました。

* サーバサイドと連動ができる( ≒ APIでPush通知の送信ができる )
* 全体通知と個別通知が存在するのでそれらのハンドリング
* Push通知の開封率調査等はしない
* できるだけ低コスト・低実装負荷

## 検討対象

前提を踏まえると、モバイルアプリを中心に考えるmBaaS系のサービスは除外して
今回以下4つ検討し、良い点と悪い点をまとめました。
※他サービスが選外だった理由は最後にまとめてます

* Firebsae (Firebase Cloud Messaging)
* AmazonSNS
* OneSignal
* 独自実装

### [Firebsae (Firebase Cloud Messaging)](https://firebase.google.com/docs/cloud-messaging/?hl=ja)

#### 良い点

* 無料
* iOS/Android/Webブラウザ、どの系統も実装化
* 上位の仕組みとしてFirebaseNotificationがあり、簡単な送信UIは用意されている

#### 悪い点

* スケーラビリティが不明瞭（まだサービス自体が他社のサービスに比べてインターネット上の知見が少なめ）
* 細かいことをFirebase **だけ** でやろうとすると制約が多い（サービス側実装で吸収もできる）

#### 関連リンク

* [第9回　Firebase Cloud MessagingとFirebase Notificationsでメッセージを送信する：スマホアプリ開発を加速する，Firebaseを使ってみよう](http://gihyo.jp/dev/serial/01/firebase/0009)
* [Firebaseをやめた4つの理由 \- Qiita](http://qiita.com/ko2ic/items/3d25d41971636a28cc1c)

### [AmazonSNS](https://aws.amazon.com/jp/sns/)

#### 良い点

* 知見が多い
* 最低限の登録SDKになっているのでカスタマイズ性高め
* AWSの枠組みの中にあるので連携の展望を考えることも可能

#### 悪い点

* 価格が従量制<br>(ただし[100万件まで無料、それ以降100万件毎に0.5$](https://aws.amazon.com/jp/sns/pricing/)なのでちゃんとIDの掃除をすれば気にならないレベルの価格帯）
* 特に分析コンソールなどはないので自前実装

#### 関連リンク

* [大規模ネイティブアプリへのプッシュ通知機能導入にあたって考えたこと \- Qiita](http://qiita.com/gomi_ningen/items/ab31aa2b3d46bb6ffa5e)


### [OneSignal](https://onesignal.com/)

#### 良い点

* 分析等の機能がセットにも関わらず無料

#### 悪い点

* iOS/Androidともに設定をSDKがまるごとやってくれるので、楽な反面カスタマイズしにくい
* あまり日本での実装例がない

#### 関連リンク

* [プッシュ通知をParseからOneSignalに移行した話 \| スペースマーケットブログ](http://blog.spacemarket.com/code/migrate-users-from-parse-to-onesignal/)


### 独自実装

#### 良い点
* Sidekiqを使ってキューで送るので実装が明白
* カスタマイズ性が高く、Appleの仕様変更等にも強い

#### 悪い点
* BaaSサービス利用に比べて実装コストは若干高め
* 規模が大きくなってきたら色々追加で仕組みを考える必要がでてくる<br>(一括送信時のキュー管理ハンドリングの仕組みを考える必要が出てくる、等）

## 選外サービス

いろいろありますが、基本的には

* 料金が月額制で上限がある
* API経由でメッセージ送信ができない
* 大量送信、即時受信がウリのところもあるが今回の条件にはあまり合わない

というところから見送ってます。
以下リンクのみ

* [Growth Push](http://growthpush.com/about_push_notification)
* [ニフティクラウド mobile backend](http://mb.cloud.nifty.com/price.htm)
* [Urban Airship](https://www.urbanairship.com/)
* [PushWoosh](https://www.pushwoosh.com/)
* [CORE ASP](http://core-asp.com/)(無料だけど展望性が不安）
* [Azure Mobile Apps](https://docs.microsoft.com/ja-jp/azure/app-service-mobile/app-service-mobile-value-prop)(Azureの枠組みなので不透明)


## 参考ページ

* [Push通知を自前でつくるか、SaaSを使うか \- Amazon SNS, Urban Airship, PushWoosh, CORE PUSH, Fello を比較してみた \- API比較\.com](http://www.apihikaku.com/entry/2016/05/12/084941)
* [Parse\.com 終了に伴う mbaas サービスの移行方法（終了直前の大幅更新！） \- Qiita](http://qiita.com/ogaoga/items/d300e64dbe923ecbdf1c)


