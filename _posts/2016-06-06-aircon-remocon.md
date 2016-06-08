---
layout: post
title:  "USB赤外線リモコンキットを使ってみよう"
date:   2016-06-06 22:00:00 +0900
categories: raspberrypi
tags: [raspberry pi, USB赤外線リモコンキット]
---
USB赤外線リモコンキットを使って、エアコンや照明のオンオフを制御してみます。

　

## USB赤外線リモコンキットって？？

簡単に言うと、家電のリモコンの代わりになるものです。

今回は、ビット・トレード・ワン製USB赤外線リモコンキットを使用します。

ビット・トレード・ワン社の製品紹介ページ : 
[http://bit-trade-one.co.jp/BTOpicture/Products/005-RS/](http://bit-trade-one.co.jp/BTOpicture/Products/005-RS/)

Amazonで購入できます。
[http://www.amazon.co.jp/dp/B00AXVHQLC](http://www.amazon.co.jp/dp/B00AXVHQLC)

　

見た目はこんな感じです。

![]({{site.baseurl}}/images/usb_remocon_003.png)

　

製品紹介ページでは、WindowsのPCから家庭用機器を操作したり、リモコンからPCを操作することができますと書いてありますが、コマンドのソースコードが公開されているので、Raspberry Piからもコマンドラインで操作することができます。


　

## USB赤外線リモコンキット制御用コマンドのインストール

### libusb のインストール

制御コマンドに必要になる libusb をインストールします。

```bash
$ sudo apt-get install libusb-1.0.0 libusb-1.0.0-dev libusb-dev
```

　

### bto_ir_cmd の作成（ビルド）

こちらのページ [https://github.com/kjmkznr/bto_ir_cmd](https://github.com/kjmkznr/bto_ir_cmd) から、git cloneもしくはDownload ZIPでダウンロードし、任意の場所に展開します。

bto_ir_cmd ディレクトリで、makeします。
bto_ir_cmd が作成されるので、/usr/local/bin に移動し、コマンド実行できるようにします。

```bash
$ cd bto_ir_cmd/
$ make
$ sudo mv bto_ir_cmd /usr/local/bin/
```

　

## bto_ir_cmd の使い方

bto_ir_cmd の使い方は非常にシンプルです！

受信コマンドで信号を覚えて、送信コマンドで送るだけです。

　

* 信号を受信する場合

```bash
$ sudo bto_ir_cmd -e -r
```

　

* 信号を送信する場合

```bash
$ sudo bto_ir_cmd -e -t xxxxxxxx
```

　

## Raspberry Piからコマンド制御

まず、リモコンの信号を受信します。

実際に、Raspberry PiにUSB赤外線リモコンをつないで、
家電のリモコンの赤外線送信部を、USB赤外線リモコンの赤外線受信部に向けて、
ターミナルから受信コマンドを実行します。

　

赤外線受信部と送信部は以下のようになっています。

![]({{site.baseurl}}/images/usb_remocon_001.png)

　

こんな感じで、リモコンのコードを読み取ってみましょう。

![]({{site.baseurl}}/images/usb_remocon_002.png)

```bash
$ sudo bto_ir_cmd -e -r
```

　

受信ができたら、送信を行います。

USB赤外線リモコンの赤外線送信部を制御したい家電に向けて、送信コマンドを実行してみましょう。

```bash
$ sudo bto_ir_cmd -e -t (上で取得した値)
```

　

私の家のリモコンは以下のようになっていました。

受信コマンドで読み取った値を送ってみても失敗することもあったので、
何度か試してみてうまく動作するものを採用しました。

```
照明全灯 022020806E01FEE70C9768000000000000000000000000000000000000000000000000
照明豆球 022020806E03FCE70C8F70000000000000000000000000000000000000000000000000
照明消灯 022020806E00FFE70C8B74000000000000000000000000000000000000000000000000

冷房 30℃ 風量しずか 風向き真ん中くらい
冷房ON  05386811DA27F00D000F11DA2700D331410000280B0892000000000000000000000000
冷房OFF 05386811DA27F00D000F11DA2700D330410000280B0891000000000000000000000000

暖房 18℃ 風量自動
暖房ON  05386811DA27F00D000F11DA2700D341410000100A0889000000000000000000000000
暖房OFF 05386811DA27F00D000F11DA2700D330410000280B0891000000000000000000000000 (冷房OFFと同じ)
```

　

各種家電のリモコンコードについての情報が、Assembly Deskさんの
pdfに少しだけ載っていたので参考になるかもしれません。

[http://a-desk.jp/project/hobby/IR/USB_IR_REMOCON_LIBRARY210.pdf](http://a-desk.jp/project/hobby/IR/USB_IR_REMOCON_LIBRARY210.pdf)

　

こちらの情報を参考にさせていただきました。

[Assembly Desk : USB接続赤外線リモコンのページ](http://a-desk.jp/modules/forum_hobby/index.php?cat_id=8)

[DSAS 開発者の部屋 : 親指サイズのUSB赤外線リモコンが面白い](http://dsas.blog.klab.org/archives/52097996.html)

