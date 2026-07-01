# Build Log

組み立て、失敗、変更、気づきを時系列で記録する。

## ログ

- 2026-06-30: `firmware/zmk-config/` に `wintinue/zmk-corne` を取り込み。取り込み元commitは `0236c3a9f466411806be85f202e3c509db495592`。
- 2026-06-30: `firmware/zmk-config/build.yaml` のboard名を `seeeduino_xiao_ble` から `xiao_ble//zmk` に更新。
- 2026-06-30: p01用ZMK configをGitHub Actionsでビルドするroot workflow `.github/workflows/p01-zmk-build.yml` を追加。
- 2026-06-30: GitHub Actions初回実行は、ZMKの再利用workflowがネストした `zmk-config` 配置を前提にしておらず `west update` で失敗。p01専用workflowに差し替え、左右のUF2だけをビルドする構成に変更。
- 2026-06-30: GitHub Actions run `28447236060` が成功。artifact `p01-zmk-firmware` に `chipper_left-xiao_ble-zmk.uf2` と `chipper_right-xiao_ble-zmk.uf2` が出ることを確認。
- 2026-07-01: JonMuller `corne-choc-xiao` のGerber/BOM/CPLを固定し、PCB + 部分PCBAを第一候補にした発注・購入チェックリスト `parts.md` を作成。
- 2026-07-01: レビュー結果を反映。`chipper.dtsi` の `diode-direction` は `col2row` でPCBA想定と一致。`J8` はPico-EZmate 1.2mm系として扱い、XIAOはdirect mount前提で発注前チェックに追加。
- 2026-07-01: 再レビュー結果を反映。`C2972761` は JKSEMI `1N4148SOD-323` と特定。`SW23` はGerberからthrough-hole SPDT slide switch footprintとして寸法メモを追加。
- 2026-07-01: JLCPCBへPCB + bottom side部分PCBAで発注。PCB 5枚、PCBA 5枚、green / 1.6mm / HASL with lead、面付け済み、different designs 2。BOM/CPL照合は `C2972761` と `C505023` の2部品だけ、配置previewで `D1`-`D21` と `J8` のbottom配置を確認済み。
- 2026-07-01: 遊舎工房でChocソケット50個、MBK 1U Black 50個、MBK 1U Homing Black 2個、Kailh Choc v1 Red Pro 70個を購入。赤軸は完売だったため、p01はRed Proで進める。
- 2026-07-01: 秋月でSeeed XIAO BLE nRF52840を3個購入。p01は左右で2個使い、残り1個はdirect mount失敗時の予備にする。
