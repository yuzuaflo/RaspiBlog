---
layout: post
title:  "Raspberry PiのOSインストール"
date:   2016-05-16 21:00:00 +0900
categories: raspberrypi
tags: [raspberry pi, raspbian]
---
Raspberry PiのOSインストール方法をまとめました。

NOOBSを使用して、Raspberry Pi 3(もしくは2)にRaspbianをインストールする手順です。

　

## 必要なものの準備
Raspberry PiのOSインストールのためには、以下のものを準備する必要があります。

* Raspberry Pi本体
* microSD (class 10/8GB以上推奨)
* HDMIケーブル
* LANケーブル
* USB電源アダプタ、microUSBケーブル(スマートフォン用のものでよい)
* ディスプレイ、マウス、キーボード

　

## OSインストール用microSDの作成

### microSDのフォーマット

microSDをFATでフォーマットします。

(exFATにしないように注意！)

　

### NOOBSのダウンロード

Raspberry Piの公式サイトのダウンロードページから、NOOBSをDownload ZIPでダウンロードします。

* 公式サイトダウンロードページ [https://www.raspberrypi.org/downloads/noobs/](https://www.raspberrypi.org/downloads/noobs/)

　

公式サイトは時間がかかることが多いので、ダウンロードがうまくいかなそうな場合は、ミラーサイトからダウンロードしてもよいかもしれません。

* JAISTのミラーサイト [http://ftp.jaist.ac.jp/pub/raspberrypi/NOOBS/images/](http://ftp.jaist.ac.jp/pub/raspberrypi/NOOBS/images/)

　

ZIPファイルを展開して、microSDのルートディレクトリにコピーします。

　

## Raspbianのインストール

作成したインストール用のmicroSDをRaspberry Piにセットします。

HDMI、キーボード、マウス、LANケーブルをRaspberry Piに接続します。

microUSBをRaspberry Piに接続します。

Raspberry Piの電源がオンになります。(Raspberry PiのLEDが点灯します。)

OSのインストール画面が表示されるので、Raspbianを選択してInstallボタンを押下します。

![Image of Install]({{site.baseurl}}/images/install_001.png)

　

## Raspbianファームウェア、パッケージのアップデート

ターミナルを起動し、以下のコマンドを実行します。

```bash
$ sudo apt-get update
$ sudo apt-get upgrade
$ sudo rpi-update
$ sudo reboot
```

　

## 日本語環境の設定

### 日本語フォントのインストール

ターミナルを起動し、以下のコマンドを実行し、日本語フォントをインストールします。

```bash
$ sudo apt-get install ttf-kochi-gothic xfonts-intl-japanese xfonts-intl-japanese-big xfonts-kaname
$ sudo reboot
```

　

### 日本語入力環境の設定

Anthyをインストールします。

```bash
$ sudo apt-get install ibus-anthy
```

　

### 自動起動の設定

.bashrc に以下の4行を追加します。

```bash
export GTK_IM_MODULE=ibus
export XMODIFIERS=@im=ibus
export QT_IM_MODULE=ibus
ibus-daemon -d -x
```

　

### Raspberry Piの設定変更

GUIメニューから以下の設定を行います。

* menu → preferences → raspberry pi configuration → localisation

* set localeの設定
  * language ja
  * country JP
  * characterset UTF-8
* set timezoneの設定
  * area asia
  * location tokyo
* set keyboardの設定
  * country japan
  * variant japanese
* wifi countryの設定
  * country jp japan

　

スクリーンショットは日本語に変更された後のものになりますが、参考までに載せておきます。

![Image of Install]({{site.baseurl}}/images/install_002.png)
![Image of Install]({{site.baseurl}}/images/install_003.png)

　

設定が終了したらRaspberry Piを再起動します。

```bash
$ sudo reboot
```

　

こちらのサイトを参考にさせていただきました！

[ツール・ラボ RaspberryPi電子工作入門](https://tool-lab.com/make-course/raspberrypi/)
