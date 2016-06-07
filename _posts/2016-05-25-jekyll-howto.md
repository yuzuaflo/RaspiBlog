---
layout: post
title:  "Jekyll + GitHub Pagesでブログサイトを作ろう"
date:   2016-05-28 16:00:00 +0900
categories: jekyll
tags: [jekyll, github pages]
---
※ 今回は、Raspberry Piに全然関係ないネタになります。

　

Jekyll + GitHub Pagesでブログサイトを作成します。

このサイトもJekyll + GitHub Pagesの組み合わせで作成されています。

　

## Jekyllとは？
[Jekyll](http://jekyllrb-ja.github.io/)を使うと
マークダウンファイルから静的Webサイトやブログを簡単に作成することができます。

　

## Jekyllのインストール
[Jekyllのインストール方法](http://jekyllrb-ja.github.io/docs/installation/)に詳細なインストール方法が記載されています。

Mac OS Xを使っている方は、Ruby/RubyGemsもすでにインストールされているので、以下のコマンドを実行するだけで簡単にインストールができます。

```bash
$ gem install jekyll
```

　

バージョンを確認します。

```bash
$ jekyll --version
jekyll 3.1.3
```

バージョンは 3.1.3 が入っています。バージョンが正しく表示できればインストール完了です。

　

## Jekyllでブログサイトのひな形を作成
jekyll newコマンドで、簡単なブログサイトのひな形を作成してくれます。

また、jekyll serveコマンドで、ローカルサーバを立ち上げて、作成したブログサイトの内容を確認できます。

jekyllsampleというサイトを作ってみます。

```bash
$ jekyll new jekyllsample
$ cd jekyllsample
$ jekyll serve
...
      Generating... 
                    done in 0.27 seconds.
    Server address: http://localhost:4000/
  Server running... press ctrl-c to stop.
```

デフォルトでは、http://localhost:4000/ にローカルサーバが立ち上がります。

ブラウザから、http://localhost:4000/ にアクセスしてみましょう。

このようなサイトが表示されます。

![Image of Install]({{site.baseurl}}/images/jekyllsample_001.png)

ローカルサーバを終了させたい場合は、ctrl-c でjekyll serveコマンドを終了させればOKです。

_config.yml 以外のファイルを更新する場合は、ローカルサーバを終了させなくても、ブラウザをリロードすれば更新が反映されます。

　

## ブログサイトの編集

jekyll newコマンドで作成された jekyllsampleディレクトリには、以下のような構成でファイル、ディレクトリが作成されています。

```bash
$ ls
_config.yml _layouts    _sass       about.md    feed.xml
_includes   _posts      _site       css         index.html
```

記事を追加したい場合には、_postsディレクトリに新たなmarkdownファイル(またはmdファイル)を追加します。

　

## GitHub Pagesで公開

Jekyllで作成されたページを、GitHub Pagesで無料で公開することができます。

ブログの作成だけなら、他のブログサービスを使用してもよいのですが、Jekyll + GitHub Pagesで作成するメリットは以下だと思います。

* 無料
* Webサーバを準備する必要がない
* 作成したサイトに広告が入らない

　

### レポジトリの作成

実際にGitHub Pagesにpushして、作成したサイトを公開してみます。

まず、GitHubにログインし、jekyllsampleレポジトリを作成します。

![Image of Install]({{site.baseurl}}/images/jekyllsample_002.png)

　

### 公開の準備

ローカルに作成されている jekyllsampleディレクトリを一旦リネームしておきます。

```bash
$ mv jekyllsample jekyllsample_tmp
```

　

git cloneで、jekyllsampleディレクトリをローカルに作成します。

```bash
$ git clone https://github.com/(GitHubユーザ名)/jekyllsample.git
Cloning into 'jekyllsample'...
...
Checking connectivity... done.
```

　

先ほど退避しておいたjekyllsample_tmpのコンテンツをjekyllsampleに移動します。

```bash
$ mv jekyllsample_tmp/* jekyllsample/
$ cd jekyllsample
```

　

jekyllsampleディレクトリ内に、.gitignore ファイルを作成し、jekyllsampleレポジトリにpushしない対象を指定します。

_.gitignore_

```
_site/
.sass-cache/
```

　

_config.yml の baseurl を編集して、github.io のアドレスにしておきます。

_\_config.yml_

```yml
baseurl: "http:/(GitHubユーザ名).github.io/jekyllsample" # the subpath of your site, e.g. /blog
```

_config.yml を編集した場合は、jekyll serveコマンドを再起動する必要があります。

　

### GitHub Pagesへの公開

gh-pages ブランチを作成します。

```bash
git checkout -b gh-pages
```

　

git add、git commit、git push で、公開完了です。

```bash
git add -A
git commit -m 'first commit'
git push origin gh-pages
```

　

http://(ユーザ名).github.io/jekyllsample/ に、作成したサイトが公開されます。

下記のリンクにジャンプすると、実際に私が作成したjekyllsampleのページを見ることができます。

[http://yuzuaflo.github.io/jekyllsample](http://yuzuaflo.github.io/jekyllsample)

　

### 公開後の編集

_config.yml の baseurl を編集して GitHub Pagesのアドレスになっているので、そのままjekyll serveコマンドを使用すると、ローカルサーバでの表示がうまくできません。

–baseurl オプションを使用して、baseurl を変更すると、ローカルサーバでも正しく表示ができます。

```bash
jekyll serve --baseurl ''
```

　

追加のコンテンツを作成した際は、git add、git commit、git push をすれば、レポジトリが更新され、サイトも更新されます。

```bash
git add -A
git commit -m 'second commit'
git push origin gh-pages
```

　

こちらの情報を参考にさせていただきました。

[第27回 GTUG Girls Meetup 「Jekyllを使ってみよう」](https://github.com/yanzm/GTUGGirlsJekyll)

 [ゆーすけべー日記 : GitHub PagesとJekyllの組み合わせの便利さを今更知って勃つ](http://blog.yusuke.be/entry/2015/11/19/101509)

