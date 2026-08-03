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
- 2026-07-14: 右手側もソケットはんだ付け・スイッチ全装着・テスターのブザー検品21キー全数合格。これで左右42キーぶんのソケットはんだは全数OK。XIAOは左右とも未装着で、次は両XIAO（`L`/`R` 記入済み・ファーム書き込み済み）のdirect mountはんだ付け → keytest.htmlでの実入力テスト。
- 2026-08-03: **p01を未完成のまま保留にする判断**（下記「保留」節を参照）。

## 保留（2026-08-03）

**XIAOのdirect mountはんだ付け直前で停止し、完成させない。** 放置ではなく判断として記録する。

**残作業**: XIAO 2個のdirect mountはんだ付けのみ（追加出費ゼロ・作業1〜2時間）。部品はすべて手元にあり、ファーム書き込み・机上split疎通テストまで完了済み。

**それでも完成させない理由**:

- **学習目的はすべて達成済み**。ZMK（shield定義・build.yaml・GitHub Actionsビルド・キーマップ）、BLE split疎通、Chocソケットのはんだ付けと導通検品42キー、JLCPCBのPCB＋部分PCBA発注（Gerber/BOM/CPL照合）は全部通した。残るdirect mountだけが未経験だが、**p03では自分のPCBにMCUを載せる設計をする**ため、既製ボードを他人のPCBに直付けする経験の転用価値は薄い
- **完成しても日常運用に投入できない**。Corne系42キーの配列は現在の運用（US 60%相当）と別物で、「1週間試しに使ってみる」ができるものではない。完成させて得られるのは達成感のみ
- 使っているChoc v1 RedとMBKキーキャップは、完成版のChoc V2（MXステム）と物理非互換のため部品としても引き継げない

**残る資産**（`docs/decisions.md` の「p01/p02の資産の活用可能性」も参照）:

- **XIAO nRF52840 が3個とも未使用のまま残る**（p01で2個使う予定だったが装着しないため）。p03でXIAO案を採る場合、ZMKボード定義の自作が丸ごと不要になる
- `tools/keytest.html`（キー確認ツール）はそのまま使える
- Chocソケット（CPG135001S30）の余りはChoc V2でも共通で流用可
- PCBは「XIAOのフットプリントと配線の実例」として参照価値がある
