---
layout: post
title:  "Raspberry Piの初期設定(VNC編)"
date:   2016-05-16 22:00:00 +0900
categories: raspberrypi
tags: [raspberry pi, raspbian, vnc]
---
Raspberry PiのOSインストールがひと通り終わりました。

ですが、このままではディスプレイやキーボード・マウスが必要で、Raspberry Piの小ささが活かせていないので、リモートアクセスの環境を整えていきます。

普段メインで使用している MacBook Pro から Raspberry Pi に、VNCを使ってリモートアクセスできるようにします。

　

## Raspberry Pi側の作業

### VNCサーバのインストール・実行

すでにRaspberry Piは無線もしくは有線でネットワークにつながっているものとします。

まずはターミナルを起動し、Raspberry PiにVNCサーバをインストールします。

```bash
$ sudo apt-get update
$ sudo apt-get install vncserver
```

　

### MACアドレスの確認

次にMACアドレスを確認します。

最終的には、Raspberry Piに割り当てられているIPアドレスが知りたいのですが、そのために、先にMACアドレスを調べておきます。

固定IPアドレスの設定をしている場合は不要です。

　

ifconfigコマンドを実行して、あらかじめMACアドレスを確認しておきます。

```bash
$ ifconfig
wlan0     Link encap:イーサネット  ハードウェアアドレス xx:xx:xx:xx:xx:xx
```

無線の場合はwlan0、有線の場合はeth0のハードウェアアドレスがMACアドレスになります。

　

## Mac側の作業

ここから先はMacでの作業になります。

　

### IPアドレスの確認

Macのターミナルを起動し、arpコマンドを実行して、同一ネットワークに接続されているデバイスを確認します。

```bash
$ arp -a
? (192.168.xx.xx) at xx:xx:xx:xx:xx:xx on en0 ifscope [ethernet]
```

先ほど調べたMACアドレスが一致する行がRaspberry Piの情報になります。

192.168.xx.xx というような値がIPアドレスです。

　

### sshで接続

sshコマンドで、Raspberry Piにアクセスできることを確認します。

```bash
$ ssh (ユーザ名)@192.168.xx.xx  ← 先ほど確認したIPアドレス
```

パスワードを入力すると、Raspberry Piにログインできます。

　

Raspberry Piにログインできたら、vncserverコマンドで起動させます。

初回のみパスワードを聞かれますので、8文字以内で入力します。

ここで設定したパスワードは、後ほどクライアント側からアクセスする際に使用します。

```bash
$ vncserver

(パスワードの設定)

New 'X' desktop is (マシン名):1

Starting applications specified in /home/pi/.vnc/xstartup
Log file is /home/pi/.vnc/(マシン名):1.log
```

X デスクトップ 1番が起動しました。
（もう一度実行すると2番が起動します）

　

### VNCクライアントで接続

Macを使用している場合、VNCクライアントソフトをインストールする必要はありません。

Finder の 移動 メニュー から サーバへ接続 を選択します。

![]({{site.baseurl}}/images/install_vnc_001.png)

　

サーバアドレス に以下のように入力します。

```
vnc://(Raspberry PiのIPアドレス):(ポート番号 + ディスプレイ番号)
```

　

例えばこんな感じで入力します。

```
vnc://192.168.xx.xx:5901
```

　

パスワードを聞かれるので入力します。

ここで入力するのは、vncserver コマンド実行時に設定したパスワードになります。

(Raspberry Piのログインパスワードではないので注意！)

　

VNCでRaspberry Piの画面が表示できました。

![]({{site.baseurl}}/images/install_vnc_002.png)

　

Windows PCの場合は、VNCクライアントソフトをインストールする必要があります。

Real VNC や Ultra VNC がメジャーなようです。

Real VNC : [https://www.realvnc.com/download/viewer/](https://www.realvnc.com/download/viewer/)

Ultra VNC : [http://www.uvnc.com/downloads/ultravnc.html](http://www.uvnc.com/downloads/ultravnc.html)

　

## 2回目以降のアクセス方法

ここまでできれば、Raspberry Piに接続されているディスプレイやキーボードなどは外しても大丈夫です。

次回からは、以下の手順でリモートアクセスができるようになります。

```
* MacのターミナルからRaspberry PiのIPアドレスを確認
* sshコマンドでRaspberry Piにログイン
* vncserverを起動
* 画面共有でRaspberry Piに接続
```

