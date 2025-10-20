# SwitchBot Plug Mini for ESPHome Changelog
**English** | [日本語](docs/CHANGELOG-ja.md)

## 1.1.1
- Removes specification of ESP32 framework version and platform version from HomeKit firmware

This version contains no functional changes and is a maintenance build to confirm compatibility with the latest ESPHome version (2025.10.2).\
Users already using SwitchBot Plug Mini with ESPHome do not need to update.

## 1.1.0
- Add `Total Monthly Energy` sensor
  - This sensor resets at 0 am on the first day of every month.

## 1.0.1
- Reduced `accuracy_decimals` (Number of decimal points) for `Power` sensor to `1`
- Increased `update_interval` for power monitoring sensor to `5s`
  - This improves the accuracy for `Power` sensor.
- Increased `update_interval` for `Apparent Power` sensor to `20s`
- Increased `update_interval` for `Power Factor` sensor to `20s`

## 1.0.0
- First release
