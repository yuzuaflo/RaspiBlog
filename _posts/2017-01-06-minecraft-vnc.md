---
layout: post
title:  "Minecraft Pi Editonで遊ぼう(VNC編)"
date:   2017-01-06 21:00:00 +0900
categories: raspberrypi
tags: [raspberry pi, minecraft, vnc]
---

[前の記事]({{site.baseurl}}{{page.previous.url}})では、Minecraft Pi Editionの基本操作を紹介しました。

　

ここまではHDMIディスプレイ、マウス、キーボードを接続して、操作する前提で書いていましたが、VNC経由でリモート操作をすることもできるので、この記事ではその方法を紹介します。

　

ただし、VNC経由でプレイすると、タイムラグがかなりあり、操作感は相当低下します。

マウス・キーボードは、直接Raspberry Piに接続することを強くオススメします。

ディスプレイもできれば直接つないだ方がよいです。

これらのことを理解した上で、どうしてもVNCでやりたいという方のみ、下記に進んでください。

　

## VNCサーバのインストール

アルファ版のVNCServerをダウンロードします。

```bash
$ curl -OL https://github.com/RealVNC/raspi-preview/releases/download/5.3.1.18206/VNC-Server-5.3.1-raspi-alpha1.deb
```

　

パッケージを展開します。

```bash
$ sudo dpkg -i VNC-Server-5.3.1-raspi-alpha1.deb
```

　

サービスモード起動と自動起動設定を行います。

```bash
$ sudo systemctl start vncserver-x11-serviced.service
$ sudo systemctl enable vncserver-x11-serviced.service
```

　

#### 注意点
自分の環境では、tightvncserverと競合してしまい、パッケージの展開の際にエラーが出てしまいました。

その場合は、tightvncserverをアンインストールしてから、再度パッケージの展開をやり直すとうまくいきました。

```bash
$ sudo apt-get remove tightvncserver
$ sudo dpkg -i VNC-Server-5.3.1-raspi-alpha1.deb
```

　

## VNCサーバの設定
/boot/config.txt を編集します。

```bash
$ vi /boot/config.txt
```

以下のように設定しました。

```
hdmi_force_hotplug = 1
hdmi_ignore_edid = 0xa5000080
hdmi_group = 2
hdmi_mode = 28
overscan_left=16
overscan_top=16
overscan_right=16
overscan_bottom=16
```

　

## VNCクライアントのインストール

私の環境はMacなので、Macの情報のみ書きます。Windowsや他のOSの方、ごめんなさい。

もともとVNCの環境は構築してあり、Macの画面共有メニュー（Finder→移動→サーバに接続）からアクセスできていました。

しかし、Minecraft Pi Edition をリモート操作するためには、画面共有メニューは使えないようです。（リモートログインはできるのですが、ゲーム画面が真っ黒になってしまいます。）

専用のVNCクライアントをインストールする必要があります。

　

RealVNCViewer をインストールしました。

[https://www.realvnc.com/download/vnc/](https://www.realvnc.com/download/vnc/)

　

## VNCクライアントの設定

VNCViewerを起動します。

（RealVNCという名前ですが、インストールされるアプリ名はVNC Viewerのようです。）

　

右クリックメニューで New Connectionを選択する。

以下の項目を設定します。

* General
  * VNC Server → VNCサーバのアドレスを入力
* Options
  * ViewOnlyにチェック
  * VNCのマウス・キーボード操作は無効になります
* Expert
  * PreferredEnoding → JPEG
  * ColorLevel → full

![]({{site.baseurl}}/images/minecraft_viewer_setting02.png)

　

リモート接続できました。

![]({{site.baseurl}}/images/minecraft_pi_vnc01.png)

　

こちらの情報を参考にさせていただきました。

[Raspberry Pi Zero のヘッドレス セットアップと MinecraftをVNC経由で出来るようにするメモ](http://qiita.com/apatchk/items/d202fc957dfcb7879f57)

