---
layout: post
title:  "Raspberry PiのOSインストール"
date:   2016-05-16 21:00:00 +0900
categories: raspberry pi 
---
Raspberry PiのOSインストール
NOOBSを使用して、Raspberry Pi 3(もしくは2)にRaspbianをインストールする手順です。

## 概要
1. 準備するもの
2. OSインストール用microSDの作成
3. Raspbianのインストール
4. Raspbianファームウェア、パッケージのアップデート
5. 日本語環境の設定

　

### 1. 準備するもの
* Raspberry Pi本体
* microSD (class 10/8GB以上推奨)
* HDMIケーブル
* LANケーブル
* USB電源アダプタ、microUSBケーブル(スマートフォン用のものでよい)
* ディスプレイ、マウス、キーボード

　

### 2. OSインストール用microSDの作成

#### microSDのフォーマット

* microSDをFATでフォーマットします。

#### NOOBSのダウンロード

* こちら↓↓のページからDownload ZIPでダウンロードします。
* [https://www.raspberrypi.org/downloads/noobs/](https://www.raspberrypi.org/downloads/noobs/)
* ZIPファイルを展開して、microSDのルートにコピーします。

　

### 3. Raspbianのインストール
* 上記の手順2 で作成したインストール用のmicroSDをRaspberry Piにセットします。
* HDMI、キーボード、マウス、LANケーブルをRaspberry Piに接続します。
* microUSBをRaspberry Piに接続します。
  * Raspberry Piの電源がオンになります。(Raspberry PiのLEDが点灯します。)
* OSのインストール画面が表示されるので、Raspbianを選択してInstallボタンを押下します。
![Image of Install]({{site.baseurl}}/images/install.png)

　

### 4. Raspbianファームウェア、パッケージのアップデート
ターミナルを起動し、以下のコマンドを実行します。

```bash
$ sudo apt-get update
$ sudo apt-get upgrade
$ sudo rpi-update
$ sudo reboot
```

　

### 5. 日本語環境の設定

#### 日本語フォントのインストール

ターミナルを起動し、以下のコマンドを実行し、日本語フォントをインストールします。

```bash
$ sudo apt-get install ttf-kochi-gothic xfonts-intl-japanese xfonts-intl-japanese-big xfonts-kaname
$ sudo reboot
```

　

#### Raspberry Piの設定変更

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

設定が終了したらRaspberry Piを再起動します。

```bash
$ sudo reboot
```

