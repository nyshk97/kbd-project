# 部品メモ

確定部品、候補部品、試作ごとの差分を記録する。

## 確定寄り

| 種別 | 部品 | 用途 | メモ |
| ---- | ---- | ---- | ---- |
| PCB | JonMuller `corne-choc-xiao` | p01 | ハードウェア正本。Gerber/BOM/CPLを使う |
| スイッチ | Kailh Choc v1 Red | 全試作共通 | ロープロファイル、リニア |
| MCU | Seeed Studio XIAO nRF52840 | p01 | 既存Corne Xiao系シールドとの互換性を優先 |
| トラックボールセンサー | PMW3610 | 後続試作、target | ZMK対応を前提 |
| エンコーダー | EVQWGD001 | 後続試作、target | 左手側のみで検証 |

## 未確定

| 種別 | 候補 | 確認したいこと |
| ---- | ---- | ---- |
| MCU | Seeed Studio XIAO nRF52840 Plus | target候補。追加GPIOが必要か、Plus用パッドをPCBで拾うか |
| キーキャップ | MBK / 自作 | 打鍵感、親指キー形状、印字方針 |
| ホットスワップソケット | Choc v1対応 | 基板セット同梱の有無 |
| ケース素材 | PLA / PETG / その他 | 強度、精度、手触り |
| バッテリー | 未定 | 容量、厚み、配置 |
| バッテリーコネクタ | JSTなど | 基板側仕様、同梱の有無 |
| 左右接続の物理構成 | 未定 | 電源スイッチ、充電、デバッグしやすさ |

## 注意メモ

- LeducH `corne-choc-xiao` はJonMuller版とGerber/BOM/CPLが同一なので、整理版・ガイド・購入リンクとして参照する
- p01のZMK configはwintinue `zmk-corne` を出発点にし、`chipper_left` / `chipper_right` を使う
- wintinue `zmk-corne` の `build.yaml` は旧board名 `seeeduino_xiao_ble` のままなので、current ZMK向けに `xiao_ble//zmk` へ変更してビルド確認する
- Choc標準18x17mmキーキャップ前提。19x19mm系キーキャップは使わない
- 無印XIAO前提の既存シールドにXIAO nRF52840 Plusを載せても、基本14ピン互換部分しか使えない
- Plusの追加GPIOを使うには、追加キャステレーションパッドまで配線したPCBが必要
- `BAT` や `AIN7_BAT` など電源系に見える名前のパッドは、必ず回路図で用途を確認してから配線する
