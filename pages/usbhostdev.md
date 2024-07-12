---
layout: page
title: USBホスト開発
permalink: /usbhostdev.html
nav_order: 600
---

# USBホスト開発

Suzuno32RVの搭載しているCH32V203C8T6はUSBホスト機能にも対応しています。まだ記事や情報が少ないですが、発展的・応用的な使い方として、USBホスト機能にも挑戦してみましょう。

本ページでは、公式サンプルコードをもとにしたUSBキーボードの入力読み取りを解説します。

**なおUSBデバイス開発で利用したTinyUSBは、本ページ執筆時点ではUSBホスト機能には対応していません。**

## 公式サンプルコード

WCH提供のサンプルコードは、EVTパッケージ内 `EVT/EXAM/USB/USBFS` にあります。"HOST_*" がUSBホスト機能のサンプル、`DEVICE/`以下がUSBデバイス機能のサンプルです。

USBキーボードの読み取り、MTPデバイス接続、USBメモリ（マスストレージ）のアクセスなどができます。

公式サンプルコードはMounRiver Studio向けのプロジェクトです。必要なファイルのコピーやC++環境向けの改変をすることで、PlatformIOでビルド可能な場合もあります。

公式サンプルコード： [https://github.com/openwch/ch32v20x/tree/main/EVT/EXAM/USB/USBFS](https://github.com/openwch/ch32v20x/tree/main/EVT/EXAM/USB/USBFS)


## Arduinoでビルド可能なUSBキーボード→シリアル出力

公式サンプルコードをわたしが改変した、USBキーボード入力を読み取って情報をシリアルへ出力するコードです。Arduino IDEでビルドできます。

[https://github.com/verylowfreq/arduino_ch32v203_uart_usbhost_keyboard](https://github.com/verylowfreq/arduino_ch32v203_uart_usbhost_keyboard)

USBでのキーボードの入力読み取りは、ホスト側（ここではSuzuno32RV）が定期的にデータを要求し、接続されたキーボードが入力の状態を返答することで行なわれます。

ポートにキーボードが直接接続された場合と、USBハブを経由している場合で処理が若干異なるため、サンプルコードでは切り分けてコードが書かれています。

なお本コードは簡易的なものであり、入力が正常に読み取れないキーボードもあります。
