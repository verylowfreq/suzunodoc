---
layout: page
title: Suzuduino UNOの注意点
permalink: /note_suzuduinouno.html
nav_order: 450
---

# Suzuduino UNOの注意点

Suzuduino UNOにのみ適用される、いくつかの注意点があります。（Suzuno32RVは該当しません）

## ピン PB8 は出力専用です

**PB8** は出力専用のピンです。入力に設定することはできますが、無効な値が読み込まれます。また、BOOTスイッチと接続されています。これは、搭載している CH32V203K8T6 の制約によります。


## ピン PD0, PD1 を利用するには追加のコードが必要です

**PD0, PD1** をGPIO（入出力）のピンとして利用するには、以下のように利用します。

```
void setup() {
    // この2行が追加で必要です。
    RCC_APB2PeriphClockCmd(RCC_APB2Periph_AFIO, ENABLE);
    GPIO_PinRemapConfig(GPIO_Remap_PD01, ENABLE);

    // 以下、通常のピンと同じように入出力を設定し、利用できます。
    pinMode(PD0, OUTPUT);
    pinMode(PD1, OUTPUT);
}
```

これは、搭載している CH32V203K8T6 の制約によります。デフォルトでは外部水晶発振子のピンとして初期化されます。
