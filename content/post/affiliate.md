---
title: "リアルアフィリエイト対応"
date: 2017-08-15T16:38:28+09:00
tags: ["アフィリエイト"]
author: "yokoo"
slug : ""
draft: true
---

リアルアフィリエイト対応
# テーブル作成

## affiliate_users
| column_name | type | note|
|:-----------------:|:------------------:|:------------------:|
| id | integer| |
| user_id | integer | |
| user_payment_id | integer | |
| tel | string | 問い合わせ用電話番号 |
| verify_code | string | 認証用コード |
| tracking_session_id| string | 各代理店のトラッキング用のセッションID（paramsとして送信される） |
| affiliate_source | integer | 広告元(1: adcrops, 2: accesstrade(アクセストレード), 3: eqsview(EQS VIEW) | 

## ユーザーフロー
1. 各代理店のLPを表示
2. メディカルノートLPに遷移
3. 「有料会員登録」をクリック
4. 決済方法選択画面表示
5. 決済登録
6. 決済登録完了（表示されている認証用コードをメモ）
7. 問い合わせ用電話番号登録（任意）
8. サインアップページ表示
9. 認証用コード/メアド/パスワードを入力して、会員登録
10. メアドアクティベート
11. ログイン

## システムフロー
1.決済登録
  ・user作成（メアドなしの空データ）
  ・user_payment作成（購入処理中）
  ・affiliate_user作成
2.決済完了
  ・ペイジェントに決済結果問い合わせ
  ・user更新（有料会員）
  ・user_payment更新（購入済/継続中）
3.サインアップ
  ・認証用コードを検索
  ・user更新（email/password）
  ・アクティベートメール送信

## URL
1. アフィリエイト用LP `/affiliate?aff_source=adcrops`
2. アフィリエイト用サインアップ `/affiliate/signup`
3. アフィリエイト用サインアップ完了 `/signup/complete`
4. アフィリエイト用決済方法選択 `/user/payment/select?aff_source=hogehoge`
5. アフィリエイト用クレジットカード情報入力 `/user/credit_settlements/new?aff_source=hogehoge`
6. アフィリエイト用決済完了 `/user/payment/complete?aff_source=hogehoge`

## トラッキング送信
### adcrops
・LP転送時のURI
`https://medicalnote-qa.jp/affiliate?aff_source=adcrops&xuid=XXXXX`

・有料登録完了ページに下記タグを設定

```
<script language=“javascript” src=“https://t.adcrops.net/ad/p/cv?_js=1&_buyer={広告主ID}&_article={キャンペーンID}&_price={売上金額}&_buid={ユーザー管理ID}"></script>
<noscript><imgsrc="https://t.adcrops.net/ad/p/cv?_buyer={広告主ID}&_article=={キャンペーンID}&_price ={売上金額}&_buid={ユーザー管理ID}" width="1" height="1"></noscript>
```
・有料解約完了ページに下記タグを設定

```
<script language=“javascript” src=“https://t.adcrops.net/ad/p/ecv?_js=1&_buyer={広告主ID}&_out=1&_xuid={セッションID}&_buid={ユーザー管理ID}"></script>
<noscript><imgsrc=“https://t.adcrops.net/ad/p/ecv?_buyer={広告主ID}&_out=1&_xuid={セッションID}&_buid={ユーザー管理ID}" width="1" height="1"></noscript>
```

広告主ID・・・代理店が発行
キャンペーンID・・・代理店が発行
売上金額・・・1登録あたりのユーザー負担金額、もしくは1と設定してください
ユーザー管理ID・・・MNが発行（ `user_id` ）

### EQS VIEW
・LP転送時のURI
`https://medicalnote-qa.jp/affiliate?aff_source=wqsview&eqsid=XXXXX`

・有料登録完了/解約完了時に下記URLにGETリクエスト

```
https://eqs-manage.appspot.com/?a1=【アカウントID】&x1=【アクションID】&d1=1&k1=u&p1=【ユニークID】&k1=session_id&p1=【セッションID】
```
アカウントID・・・代理店が発行
アクションID・・・代理店が発行
ユニークID・・・ MNが発行（ `user_id` ）
セッションID・・・クリック時のパラメータ内の `eqsid`

### アクセストレード
・LP転送時のURI
`https://medicalnote-qa.jp/affiliate?aff_source=accesstrade&rk=XXXXX`
