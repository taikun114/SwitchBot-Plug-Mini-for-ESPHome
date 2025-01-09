# SwitchBot プラグミニ for ESPHome 変更ログ
[English](/CHANGELOG.md) | **日本語**

## 1.1.0
- `Total Monthly Energy`（一ヵ月の累計消費電力量）センサーを追加
  - このセンサーは、毎月1日の午前0時にリセットされます。

## 1.0.1
- `Power`センサーの`accuracy_decimals`（小数点以下の数）を`1`に減少
- 電力モニタリングセンサーの`update_interval`を`5s`に増加
  - これにより、`Power`（電力）センサーの精度が向上します。
- `Apparent Power`（皮相電力）センサーの`update_interval`を`20s`に増加
- `Power Factor`（力率）センサーの`update_interval`を`20s`に増加

## 1.0.0
- 最初のリリース
