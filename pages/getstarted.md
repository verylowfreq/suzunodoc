---
layout: page
title: まずは使ってみよう
permalink: /getstarted.html
nav_order: 50
---

# まずは使ってみよう

Suzuno32RV / Suzuduino UNOを使って、Lチカをしてみましょう。PA5ピンにLEDが実装されています。


## 必要なもの

 - Windowsパソコン
 - Suzuno32RV / Suzuduino UNO
 - USB Type-Cケーブル

本ドキュメントでは**Windows環境を対象**とします。

USBケーブルは**データ通信可能なものか**をよくご確認ください。※充電専用のType-Cケーブルも流通しているようです。


## Arduino IDEの準備

Arduino IDEをダウンロード、インストールしてください。

Arduino IDEを起動したら、「基本設定」→「追加のボードマネージャーのURL」に、以下のURLを追加してください。

`https://raw.githubusercontent.com/verylowfreq/board_manager_ch32/main/package_ch32v_index_sz.json`

ボードマネージャーで "ch32" を検索して、"CH32V Boards by M.S."をインストールしてください。

## USBドライバの準備

WindowsでArduino IDEからUSB書き込みをするために、zadigというツールでデバイスドライバの設定をする必要があります。このツールは、特定のUSBデバイスで利用するデバイスドライバの設定を上書きするものです。

**開発に利用するポートをひとつ決めて**、Suzuno32RV/Suzuduino UNOをUSB書き込み待機モードでパソコンに接続します。BOOTスイッチを押しながらボードをパソコンに接続してください。

zadig をダウンロードし、実行してください。

zadig: [https://zadig.akeo.ie/](https://zadig.akeo.ie/)

"Options" → "List All Devices" にチェックを入れてください。

USB ID **"4348" "55E0"** のデバイスを選択してください。環境にもよりますが "USB Module"や"CH375"という名前のデバイスを確認してみてください。**誤った選択をするとそのデバイスが動かなくなります。よくご確認ください。**

<img alt="" src="images/zadig_preinstall.png">

zadigウィンドウ内の矢印の**みぎ側**のリストボックスで、"WinUSB"が選択されていることを確認してください。

"Replace Driver"ボタンを押します。処理には数分かかる場合があります。

zadigウィンドウ内の矢印の**ひだり側**の表示が "WinUSB" となっていれば、成功です。

<img alt="" src="images/zadig_postinstall.png">

なお、開発に利用するUSBポートを変更した場合は、zadigでのデバイスドライバの設定をやり直してください。


## コンパイルのオプションを選択する

「ツール」→「ボード」→「CH32V Boards by M.S.」→「CH32V20x」を選択してください。
「ツール」→「Board select」から、"Suzuno32RV/SuzuduinoUNO"を選択してください。
「ツール」→「Upload method」→「WCH-ISP」を選択してください。


## コードを書く

以下のコードを入力してください。

```
#define PIN_LED PA5

void setup() {
    pinMode(PIN_LED, OUTPUT);
}

void loop() {
    digitalWrite(PIN_LED, HIGH);
    delay(500);
    digitalWrite(PIN_LED, LOW);
    delay(500);
}
```

## 書き込み・実行

まずはArduino IDEの「検証」（大きなチェックマーク）でコードに間違いがないかチェックします。

USB経由で書き込みます。以下の手順でボードをUSB書き込み待機モードにします。

 1. RESETスイッチを押します（離さないでください）
 1. BOOTスイッチを押します（離さないでください）
 1. 1秒後、RESETスイッチを離します
 1. 1秒後、BOOTスイッチを離します

Arduino IDEから「書き込み」（大きな右矢印）で、コードを書きこみます。

LEDが点滅すれば、成功です！

まれに自動でコードの実行がはじまらない場合があります。RESETスイッチを手動で押してみてください。

書き込みがエラーになる場合は、zadigを設定したポートに接続されているか確認し、USB書き込み待機モードの手順をあらためて試してみてください。
