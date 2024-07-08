---
layout: page
title: USBデバイス開発
permalink: /usbdevicedev.html
nav_order: 500
---


# USBデバイス開発

Suzuno32RV / Suzuduino UNOはUSBデバイスとして振舞うことができます。たとえばキーボードやマウスといった入力デバイス、MIDIデバイス、マスストレージデバイス、シリアル通信デバイスになることができます。公式サンプルコードと、TinyUSBライブラリが利用できます。

Arduino IDEでTinyUSBを利用するには、「ツール」→「USB support」から「Adafruit TinyUSB with USBD」を選択してください。

USBデバイス機能が暴走したなどの緊急時には、USB書き込み待機モードでリセットしてください。


## USBシリアル

Arduino LeonardoやUNO R4のように、USB接続上でシリアル通信をすることができます。

Arduino IDEでボードを正しく選択したあと、「ツール」→「USB support」→「USBD」を選択します。これで `Serial` が自動的にUSBシリアルへと置き換わります。

```
#include <Adafruit_TinyUSB.h>

void setup() {
  // 動作確認用のLED
  pinMode(PA5, OUTPUT);
  // USB有効時、これはUSBシリアルになる
  Serial.begin(115200);
  // Serial1はPA9,PA10ピンに引き出されたシリアルポート
  Serial1.begin(115200);
}

void loop() {
  digitalWrite(PA5, HIGH);
  delay(100);
  digitalWrite(PA5, LOW);
  Serial.println("Hello from USB Serial");
  Serial1.println("Hello from Hardware Serial (PA9, PA10)");
  delay(1000);
}
```


## USB入力デバイス（TinyUSB利用）

```
#include "Adafruit_TinyUSB.h"

#define PIN_SWITCH_1 PA15
#define PIN_SWITCH_2 PB3
#define PIN_SWITCH_3 PB4
#define PIN_SWITCH_4 PB5


// TinyUSBのテンプレートを利用したHIDレポートディスクリプタ
uint8_t const desc_hid_report[] = {
    TUD_HID_REPORT_DESC_KEYBOARD()
};

Adafruit_USBD_HID usb_hid;


void setup() {
  Serial1.begin(115200);
  Serial1.println("Initializing...");
  Serial1.println("USB keyboard sample code with TinyUSB");

  pinMode(PIN_SWITCH_1, INPUT_PULLUP);
  pinMode(PIN_SWITCH_2, INPUT_PULLUP);
  pinMode(PIN_SWITCH_3, INPUT_PULLUP);
  pinMode(PIN_SWITCH_4, INPUT_PULLUP);

  // Setup HID
  usb_hid.setBootProtocol(HID_ITF_PROTOCOL_KEYBOARD);
  usb_hid.setPollInterval(10);
  usb_hid.setReportDescriptor(desc_hid_report, sizeof(desc_hid_report));
  usb_hid.setStringDescriptor("TinyUSB Keyboard");
  // usb_hid.setReportCallback(NULL, hid_report_callback);
  usb_hid.begin();

  Serial1.println("Ready.");
}


void loop() {

  if (!TinyUSBDevice.mounted()) {
    // まだUSB接続されていない場合は処理をスキップ
    return;
  }

  if (!usb_hid.ready()) {
    // 前回のHIDレポートの送信が終わっていない場合は処理をスキップ
    return;
  }

  // HIDレポートディスクリプタでReportIDを指定していた場合は、その番号。無指定ならば 0
  uint8_t hid_report_id = 0;
  // 修飾キーの状態 (Ctrl, Alt, Shift, GUI)
  uint8_t modifiers = 0;
  // 押されているキーのキーコード (Usage ID。最大6キー)
  uint8_t keycodes[6] = { 0 };

  keycodes[0] = digitalRead(PIN_SWITCH_1) == LOW ? HID_KEY_A : 0;
  keycodes[1] = digitalRead(PIN_SWITCH_2) == LOW ? HID_KEY_B : 0;
  keycodes[2] = digitalRead(PIN_SWITCH_3) == LOW ? HID_KEY_C : 0;
  keycodes[3] = digitalRead(PIN_SWITCH_4) == LOW ? HID_KEY_D : 0;

  usb_hid.keyboardReport(hid_report_id, modifiers, keycodes);
}

// void hid_report_callback(uint8_t report_id, hid_report_type_t report_type, uint8_t const* buffer, uint16_t bufsize) {

// }
```

USB入力デバイスは HID という仕様で動作します (Human Interface Deviceクラス)。HIDのデバイスは、HIDレポートという単位でパソコンと情報をやりとりし、そのレポートの内容はHIDレポートディスクリプタというバイナリで定義します。
USBキーボードの場合、典型的には8バイトのHIDレポートを用いてパソコンへ入力の状態を送信します。これに対応するHIDレポートディスクリプタはTinyUSBに用意されています。

USBキーボードのHIDレポートは、
 - 修飾キーの状態（1バイト）
 - 固定値 0x00
 - 入力キー 1
 - 入力キー 2
 - 入力キー 3
 - 入力キー 4
 - 入力キー 5
 - 入力キー 6
という構造です。「いま入力されているキー」のキーコード (Usage ID) を送信します。キー入力を表さない値として 0x00 で穴埋めができます。


## USB入力デバイス（公式サンプルをベースに）

公式サンプルコードでも同様に、HIDレポートを送信することでキーボード入力が実現できます。

公式サンプルコードはMounRiver Studioで利用することが前提のC言語コードなので、Arduino IDEでもコンパイルできるように修正します。

HIDまわりの仕様についてはTinyUSBと同じです。ただし設定用の関数などは用意されていないため、USBまわりの挙動は各種のディスクリプタを直接書き換えてください。

コンパイル時にはArduino IDEの「USB support」は「None」にしてください。

コードはGitHubにて公開しています。

[https://github.com/verylowfreq/arduino_ch32v203_usbdevice_keyboard](https://github.com/verylowfreq/arduino_ch32v203_usbdevice_keyboard)
