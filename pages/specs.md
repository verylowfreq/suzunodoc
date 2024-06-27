---
layout: page
title: 基本仕様
permalink: /specs.html
nav_order: 100
---


# 基本仕様

## 共通仕様

| | |
|---|---|---|
| メインチップ | WCH CH32V203 | |
| - CPU | 32bit RISC-V RV32IMAC 144MHz(最大) | |
| - FlashROM | 64KB (+160KB) | 追加の160KB領域の利用はサンプルコードを参照 |
| - RAM | 20KB | |
| 電源 | USB (5V) | |
| ファームウェア書き込み | USB Type-C、WCH LinkEケーブル | USB書き込み待機にするには、BOOTボタンを押しながらリセットします |

## Suzuno32RV

| | |
|---|---|---|
| メインチップ | CH32V203C8T6 | LQFP 48pin |
| 水晶発振子 | 8MHz | | 
| USBホスト機能 | 対応 | Type-AコネクタにUSBHDペリフェラルが結線 | 


## Suzuduino UNO

| | |
|---|---|---|
| メインチップ | CH32V203K8T6 | LQFP 32pin |
| 水晶発振子 | なし | | 
| USBホスト機能 | なし | |
| 追加の電源 | DCジャック（センタープラス、5V-12V）| 電源の切り替えは、ジャンパ線を差し替えてください|
