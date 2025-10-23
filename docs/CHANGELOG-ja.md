# SwitchBot プラグミニ for ESPHome 変更ログ
[English](/CHANGELOG.md) | **日本語**

## 1.2.0
- `LED Brightness`（LEDの明るさ）の`オフ`（または`Off`）オプション名を`LEDオフ`（または`LED Off`）に変更
  - **これは破壊的変更です。**\
    `オフ`（または`Off`）を検出したり、切り替えたりするオートメーションを使用している場合、ファームウェアの更新に合わせてオートメーションも`LEDオフ`（または`LED Off`）に変更する必要があります。
  - この変更によって、初回インストール後にESPHomeで完全な制御を得た時、バリデーションが通らない問題（[**#4**](https://github.com/taikun114/SwitchBot-Plug-Mini-for-ESPHome/issues/4)）が修正される可能性があります。

## 1.1.1
- HomeKitファームウェアからESP32のフレームワークバージョンとプラットフォームバージョンの指定を削除

このバージョンは機能に変更はなく、最新バージョンのESPHome（2025.10.2）でビルドできることを確認するためのメンテナンスビルドです。\
すでにESPHomeでSwitchBot プラグミニをお使いの方は更新する必要はありません。

## 1.1.0
- `Total Monthly Energy`（一ヵ月の累計消費電力量）センサーを追加
  - このセンサーは、毎月1日の午前0時にリセットされます。

## 1.0.1
- `Power`（電力）センサーの`accuracy_decimals`（小数点以下の数）を`1`に減少
- 電力モニタリングセンサーの`update_interval`を`5s`に増加
  - これにより、`Power`（電力）センサーの精度が向上します。
- `Apparent Power`（皮相電力）センサーの`update_interval`を`20s`に増加
- `Power Factor`（力率）センサーの`update_interval`を`20s`に増加

## 1.0.0
- 最初のリリース
