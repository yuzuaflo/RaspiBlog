---
layout: post
title:  "温湿度センサを使ってみよう(AE-BME280)"
date:   2016-05-17 21:00:00 +0900
categories: raspberrypi
tags: raspberrypi i2c breadboard
---
Raspberry Piに温湿度センサモジュールをつなげてみます。

温湿度センサは秋月電子製のAE-BME280モジュールを使用します。

[AE-BME280](http://akizukidenshi.com/catalog/g/gK-09421/)

[データシート](http://akizukidenshi.com/download/ds/akizuki/AE-BME280_manu_v1.1.pdf)

このモジュールを使うと、温度・湿度・気圧を取得することができます。

通信方式は、I2C、SPI(3線、4線)から選択できますが、
今回はI2Cを使用することにします。

(一度はんだ付けをするとI2C ⇔ SPIには変更できないので注意してください。)

　

## 準備するもの

* AE-BME280モジュール
* はんだごてセット一式
* ブレッドボード
* ジャンパワイヤ(オスオス、オスメス)

　

## はんだ付け　

AE-BME280モジュールは、はんだ付けが必要です。

今回はI2Cを使用するので、[データシート](http://akizukidenshi.com/download/ds/akizuki/AE-BME280_manu_v1.1.pdf)の「I2Cの接続方法」説明に従い、
J3をジャンパ接続します。

また、ピンヘッダ(6箇所)をはんだ付けします。

　

![]({{site.baseurl}}/images/temperature-senser_001.png)

オモテ面

![]({{site.baseurl}}/images/temperature-senser_002.png)

ウラ面

　

## ブレッドボードに接続

下の画像のように、Raspberry Piと温湿度センサモジュールを、ブレッドボードに接続します。

![]({{site.baseurl}}/images/temperature-senser_003.png)

実際に接続するとこんな感じです！

![]({{site.baseurl}}/images/temperature-senser_004.png)

　

## I2C接続ができているかの確認

### I2Cを有効にする

GUIメニューから以下の設定を確認します。

* menu → preferences → raspberry pi configuration → interfaces
  * I2CがEnabledになっていることを確認します。
  * 変更した場合はRaspberry Piを再起動する必要があります。

　

### 必要なモジュールのインストール

i2c-tools、python-smbus をインストールします。

```bash
$ sudo apt-get install i2c-tools
$ sudo apt-get python-smbus
```

　

i2cdetectコマンドでI2Cで接続されているデバイスのアドレスを確認できます。

```bash
$ sudo i2cdetect -y 1
     0  1  2  3  4  5  6  7  8  9  a  b  c  d  e  f
00:          -- -- -- -- -- -- -- -- -- -- -- -- -- 
10: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- 
20: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- 
30: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- 
40: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- 
50: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- 
60: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- 
70: -- -- -- -- -- -- 76 -- 
```

0x76にデバイスが接続されています。

* i2cdetectコマンドのyオプションの引数は1としていますが、Raspberry Pi 1の場合は0とするようです。

　

## 温度・湿度・気圧の取得

準備ができたら、いよいよデータを取得します。
デバイスドライバをゴリゴリ書くのもよいですが、時間がないので、ここではスイッチサイエンスさんのソースコードを使わせていただくことにします。

[SWITCHSCIENCE/BME280](https://github.com/SWITCHSCIENCE/BME280)

こちらのサイトから、プログラムをダウンロードして、その中にある
python27/bme280_sample.py を使用します。

Raspberry Pi内の任意のフォルダに bme280_sample.py を作成して、以下のように実行します。

```bash
$ python bme280_sample.py
temp : 26.72  ℃
pressure : 1020.46 hPa
hum :  37.85 ％
```

温度・湿度・気圧が取得できました！


　

こちらのサイトを参考にさせていただきました！

[IT女子のラズベリーパイ入門奮闘記](http://deviceplus.jp/hobby/raspberrypi_entry_039/)
