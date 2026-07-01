# ロードマップ

最終形を一度で作らず、試作ごとに検証対象を絞る。
p02以降の試作ディレクトリは先に作らず、p01完了後に必要になった時点で追加する。

## p01-simple-split

目的: ZMK、Choc v1、左右分割、親指キーの基本フローを掴む。

最初にやること:

- ハードウェア正本を `JonMuller/gerbers` の `corne-choc-xiao` にする
- `LeducH/corne-choc-xiao` は整理版・ガイド・購入リンクの参考にする
- ファームウェア出発点を `wintinue/zmk-corne` にする
- repo URL、commit、Gerber、BOM、ZMK shield名を記録する
- boardはcurrent ZMKの `xiao_ble//zmk` 前提で確認する
- shield名は `wintinue/zmk-corne` の `chipper_left` / `chipper_right` を起点にする
- `build.yaml` の旧board名 `seeeduino_xiao_ble` を `xiao_ble//zmk` に直してビルド確認する。これは完了済み
- JonMullerのGerber/BOM/CPLを固定し、PCB + 部分PCBAを第一候補にした発注チェックリストを作る。これは完了済み
- JLCPCBへPCB 5枚 + bottom side部分PCBA 5枚を発注する。これは完了済み

残すもの:

- 左右分割
- ZMK
- Seeed Studio XIAO nRF52840
- Choc v1
- 親指キー
- 42キー前後のレイヤー運用

削るもの:

- BLEドングル
- トラックボール
- エンコーダー
- XIAO nRF52840 Plus
- OLED / RGB
- 独自キーキャップ
- 複雑なケース、テンティング

## 次の候補: wireless-dongle

目的: 最終形で使うBLEドングル構成を検証する。

- 左右BLE分割
- PC接続用ドングル
- スリープ復帰、ペアリング、遅延の確認

## 次の候補: encoder

目的: EVQWGD001エンコーダーとZMK設定を検証する。

- 左手側のみエンコーダーを追加
- レイヤーごとの動作割り当てを確認
- ケースやPCB上の干渉を確認

## 次の候補: trackball

目的: PMW3610トラックボールとAuto Mouseレイヤーを検証する。

- PMW3610
- 34mmボール
- 右手親指エリアの位置確認
- クリック、スクロール、センサー高さ、ケース支持機構の確認

## target

目的: 検証済み要素を統合して理想形に近づける。

- 左右分割
- Choc v1
- ZMK
- XIAO nRF52840 Plus候補
- BLEドングル
- EVQWGD001
- PMW3610
- 3Dプリントケース
- キーキャップ方針
