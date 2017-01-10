---
layout: post
title:  "Minecraft Pi Editonで遊ぼう(基本操作編)"
date:   2017-01-05 21:00:00 +0900
categories: raspberrypi
tags: [raspberry pi, minecraft]
---

Raspbian OSには、Minecraft Pi Editonがプリインストールされています。

![]({{site.baseurl}}/images/minecraft_pi_start.png)

　

## Minecraft Pi Edtionとは？

Minecraftは世界中で流行したゲームですが、Raspberry Piではなんと無料で遊ぶことができます！

ただし、プログラミング学習目的で提供されているものであり、ゲーム的な要素はありません。

（ちょっと残念）

　

一般的なMinecraftでは、マウスとキーボードを使ってブロックを積み上げることができますが、
Minecraft Pi Editionでは、Pythonのプログラムでブロックを配置することもできます。

![]({{site.baseurl}}/images/minecraft_pi_play01.png)

　

## Minecraft Pi Editonの始め方

* Menu → ゲーム → Minecraft Pi を選択する。

* スタート画面が表示されるので、Start Game を選択する。

* Select world 画面が表示されるのでworld を選択する。

* ゲームの初期画面が表示されます。

　

## マウスとキーボードでの操作方法

キーボードでプレイヤー位置の調整、マウスでプレイヤー視点の調整を行います。

| キー/マウス/ボタン | 動作 |
|:--:|--|
| W | 前進 |
| A | 左 |
| S | 後退 |
| D | 右 |
| E | ブロックの一覧 |
| 1〜8 | アイテム選択 |
| スペース | ジャンプ・上昇 |
| Shift | しゃがむ・下降 |
| スペース2度押し | 浮遊・落下切り替え |
| Esc | 一時停止・メニュー |
| Tab | マウスカーソルを解放 |
| マウス操作 | 視点の移動 |
| マウス右クリック | ブロックを置く |
| マウス左クリック | ブロックを壊す |
{:.mbtablestyle}

　

## Pythonプログラムを実行してみる

### ターミナルの起動

ディスプレイを直接接続している場合は、LXTerminalを起動する。

VNCでリモートから実行している場合は、VNCクライアント側のターミナルソフトを起動する。

Minecraftの画面の横に並べて、プログラムを実行できるようにします。

　

### ディレクトリの作成

Pythonスクリプト用のディレクトリを作成しておきます。

　

### Minecraft APIについて

Pythonのライブラリを使って、Minecraftを操作することができます。

以下のようなものがあります。

| API名 | 引数 | 内容 |
|--|:--:|--|
| postToChat | "文字列" | チャットメッセージの表示 |
| player.getPos | なし | プレイヤー位置の取得 |
| player.setPos | x,y,z | プレイヤー位置の移動 |
| setBlock | x,y,z,ID | 指定した座標にブロックを配置する |
| setBlocks | x1,y1,z1,x2,y2,z2,ID | 指定した座標空間にブロックを配置する |
{:.mbtablestyle}

　

(ここから先は後日記載します)

### メッセージを表示してみる


### ブロックを置いてみる

### 砂ブロックを使ってみる

### 水ブロックを使ってみる


### プレイヤー位置の取得


## 複数人でプレーしてみる
