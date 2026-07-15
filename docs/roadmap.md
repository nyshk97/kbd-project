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

## 次の候補: p02-auto-kdk

目的: Auto-KDKで設計した1台で、target構成の主要素をまとめて検証する。
（2026-07-15にAuto-KDK採用を決定し、旧候補のwireless-dongle・encoder・trackballの3試作をこの1台に統合。encoderは搭載しない方針に変更。docs/decisions.md 参照）

- Auto-KDKで設計（レイアウトはsplit-us60pct.jsonを起点に調整。右手はトラックボール使用で14ピン上限ちょうどのためキー追加不可）
- 専用無線コントローラボード（BOOTHのぎけす屋、2個セット¥9,000）
- Choc V2 19mmピッチ。軸は軽めリニアを複数買って打ち比べてから確定
- PAW3222トラックボール（右手。thumb-25/34は実配置で選ぶ）
- BLEドングル構成の検証（生成ZMK設定の対応可否を発注前に確認）
- 3Dプリントケース（生成STLをJLC3DP等へ）
- キーキャップはYUZU（PFFプロファイル）で注文（レイアウト確定後。docs/decisions.md 参照）
- 発注前チェック: 生成データのライセンス、ホットスワップソケット有無（BOM確認）、Choc V2対応ロープロスタビの入手性（2U/2.25Uキーを使う場合）

## target

目的: p02で検証したパラメータ（軸、ボール径、レイアウト、ケース角度）をAuto-KDK設定JSONに反映して作り直し、理想形に仕上げる。

Auto-KDKは再設計コストが低いため、p02とtargetの境界は厳密にしない。p02で満足すればそれをtargetとしてよい。

- 左右分割
- Choc V2 19mm（軸はp02で確定）
- ZMK（ZMK Studio）
- Auto-KDK専用無線ボード
- BLEドングル
- PAW3222トラックボール
- 3Dプリントケース（フェーズ2素材はその後検討）
- キーキャップはYUZU注文（PFFプロファイル、印字あり）
