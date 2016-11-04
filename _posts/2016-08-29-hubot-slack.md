---
layout: post
title:  "HubotでSlackのBotを作ろう(Hubotインストール)"
date:   2016-08-29 21:00:00 +0900
categories: raspberrypi
tags: [raspberry pi, slack, hubot]
---

Slackに特定のメッセージを送ると、Hubotが反応して、Slackにメッセージを返信してくれるシステムを作成します。

　

### Slack、Hubotとは？

#### Slackについて

Slack社が提供しているチームコミュニケーションツール。

1対1やグループチャットのできるWebサービスです。

PCやスマートフォンを使って、Webブラウザ上からでも、アプリケーションでも使用できます。

　

_Slack（公式サイト）_

[https://slack.com/](https://slack.com/)

![]({{site.baseurl}}/images/hubot_slack_001.png)

画像はスマートフォンのSlackアプリのスクリーンショットです。

　

#### Hubotについて

botとは、機械による自動発言システムです。
特定の時間に自動で発言したり、特定のキーワードに反応してメッセージを送ったりするものなどがあります。

Hubotは、GitHub社が開発したbotを作成するためのフレームワークです。

SlackやHipChatやChatWorkなど様々なチャットツールに対応しています。

MITライセンスで公開されています。

　

_Hubot（公式サイト）_

[https://hubot.github.com/](https://hubot.github.com/)

　

今回は、Hubotを使ってSlackのbotを作成します。

　

## Slackのチーム、アカウント作成、アプリのインストール

まずはSlackの公式サイトにアクセスして、チームの作成を行います。

流れとしては、チームを作成後、アカウントの作成を行います。

スマートフォンアプリもインストールしておきます。

下記のサイトなどを参考にしながらやってみました。

　

_Slackを導入する方法(Qiita)_

[http://qiita.com/hhyuga201515/items/d072b886dd914d4a2a4a](http://qiita.com/hhyuga201515/items/d072b886dd914d4a2a4a)

　

## Hubotのインストール

Hubotをインストールする前に、Node.jsをインストールする必要があります。

ここでは、Node.js本体のインストールの前にnodebrewをインストールして、Node.jsのバージョン管理をしやすくしています。

　

### Node.jsのインストール

nodebrewのインストール

```bash
$ curl -L git.io/nodebrew | perl - setup
```

　

.bashrc ファイルにnodebrewのパスを追加

```bash
$ echo $SHELL
$ ls -al
$ vi .bashrc
export PATH=$HOME/.nodebrew/current/bin:$PATH を一番下の行に追加
$ source ~/.bashrc
$ nodebrew help
$ nodebrew install-binary stable
$ nodebrew use stable
$ node -v
v6.4.0
```

nodeのバージョンが表示されればOKです。

　

### Hubot、CoffeeScript等のインストール

Node.jsがインストールされるとnpmコマンドが使えるようになっているはずです。

以下のコマンドで、Hubotをインストールします。
Hubotの動作に必要なcoffee-scriptと、ひな形作成ツールのyo、hubot用ひな形作成ツールのgenerator-hubotもインストールしておきます。

(-gはグローバルインストールです。)

　

```bash
$ npm -g yo hubot generator-hubot coffee-script
```

　

## Hubotの作成

準備ができたので、早速Hubotを作成してみます。

まず、Hubot用のディレクトリを作成し、移動します。

samplebotと言う名前のHubotを作成します。

```bash
$ mkdir hubot
$ mkdir samplebot
$ cd hubot/samplebot
```

　

samplebotディレクトリで、yoコマンドを実行し、Hubotを動作させる準備をします。

```bash
$ yo hubot
                     _____________________________  
                    /                             \ 
   //\              |      Extracting input for    |
  ////\    _____    |   self-replication process   |
 //////\  /_____\   \                             / 
 ======= |[^_/\_]|   /----------------------------  
  |   | _|___@@__|__                                
  +===+/  ///     \_\                               
   | |_\ /// HUBOT/\\                             
   |___/\//      /  \\                            
         \      /   +---+                            
          \____/    |   |                            
           | //|    +===+                            
            \//      |xx|                            

```

　

色々聞かれるので入力する必要があります。

Bot adapter(Botが連携するサービス)にslackを指定します。

```bash
?Owner: (入力しなくてもよい) <example@mail.com>
?Bot name: samplebot
?Description: (入力しなくてもよい)
?Bot adapter: slack
```

　

Hubotの雛形が作成されました。
ディレクトリ構成は以下のようになっています。

```bash
$ tree -L 1 ./
./
├── Procfile
├── README.md
├── bin
├── external-scripts.json
├── node_modules
├── package.json
└── scripts
```

　

で、はいよいよHubotを実行してみます。
実行ファイルは bin/hubot になります。

```bash
$ bin/hubot
```

　

プロンプトが出たら、pingコマンドを実行してみます。

(ここではまだslackではなくて、CUI上での実行です。)

```bash
samplebot> samplebot ping
samplebot> PONG
```

PONG が返ってきました。

気をつけなければいけないのは、samplebotさんに向けてpingを送ることです。

(pingだけだとPONGが返ってきません。)

ctrl + c でHubotをいったん終了します。

　

scriptsディレクトリの中に、スクリプトファイルが入っています。

scripts/example.coffee の中身を確認してみましょう。

初期状態では全てコメントアウトされているので、13〜14行目のコメントを外してみます。

```java
module.exports = (robot) ->

   robot.hear /badger/i, (res) ->
     res.send "Badgers? BADGERS? WE DON'T NEED NO STINKIN BADGERS"
  #
  # robot.respond /open the (.*) doors/i, (res) ->
  #   doorType = res.match[1]
  #   if doorType is "pod bay"
  #     res.reply "I'm afraid I can't let you do that."
  #   else
  #     res.reply "Opening #{doorType} doors"
  #
```

　

再度Hubotを起動して、badgerコマンドを実行してみます。

```bash
$ bin/hubot
...
samplebot> badger
samplebot> Badgers? BADGERS? WE DON'T NEED NO STINKIN BADGERS
```

今度はsamplebotに向けて送らなくてもレスポンスが返ってきます。
(hearとrespondの違いです。)

　

次の記事では、実際にSlackのサービスとHubotを連携させてみます。
