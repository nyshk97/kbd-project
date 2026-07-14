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
- 2026-07-11: 注文した部品がすべて到着。JLCPCBのPCB + 部分PCBA（`D1`-`D21`、`J8` 実装済み）、Chocソケット、スイッチ、キーキャップ、XIAO。次はXIAO単体の書き込みテストから組み立てに入る。
- 2026-07-12: XIAO 1個目（serial `B687D5B1468A18A7`）の書き込みテスト成功。RSTダブルタップで `XIAO-SENSE` マウント（UF2 Bootloader 0.6.1 / SoftDevice S140 7.3.0）→ `chipper_left-xiao_ble-zmk.uf2` をコピー → USB上で `Chipper`（VID 0x1d50 / PID 0x615e）のHIDキーボードとして認識。工場出荷ファームは1200bpsタッチ非対応で、ブートローダ移行は物理RSTダブルタップが必要。
- 2026-07-12: XIAO 2個目の書き込みテスト成功。同じ手順で `chipper_right-xiao_ble-zmk.uf2` を書き込み、USB上で `Chipper Right` として認識。個体に油性ペンでL/Rを記入済み。3個目は予備として未書き込みのまま。
- 2026-07-13: はんだ付け前の机上split疎通テスト成功。左XIAOをPCへUSB接続（central、`Chipper` として認識）、右XIAOを別ポートで給電し、右XIAOの `0`（P0.02 / Row0）と `6`（P1.11 / Col0）のパッドをピンセットで短絡 → PCに `y` が入力された。右のキースキャン → BLE split → 左central → USB HIDの全経路が動作。ジャンパー/ピンセット短絡はRC(0,6)=`Y` に対応（右はcol-offset 6）。左単体も `0`×`6`（Col5）短絡で RC(0,5)=`T` の入力を確認。左右ともスキャン動作OK、組み立て前のファーム・無線・HID検証はすべて完了。
- 2026-07-13: テスターとしてHIOKI 3244-60（カードハイテスタ）を購入。PCB検品・ソケットはんだ付け後の導通確認に使う。
- 2026-07-14: 左手側PCBのChocソケットはんだ付け完了。HIOKI 3244-60も到着。次はテスターでソケットの導通確認をしてから、スイッチを挿して左手側の実キー入力テストに進む。
- 2026-07-14: 左手側にスイッチを全装着し、HIOKI 3244-60の導通ブザーで全21キーを検品。各ソケットの2つのはんだ点にプローブを当ててスイッチを押すと鳴る・離すと止まるを全キーで確認、ソケットはんだは全数OK。キー確認用ツール `tools/keytest.html`（押したキーが緑に光るローカルページ）を追加。次は左XIAOのdirect mountはんだ付け。Reverso基板のため取り付け面をシルクで要確認。
