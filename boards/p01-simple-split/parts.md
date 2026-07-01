# p01 発注・購入チェックリスト

調査日: 2026-07-01

## 結論

p01は `JonMuller/gerbers` の `corne-choc-xiao` を正本にして、JLCPCBでPCBを発注する。

発注方針は **PCB + 部分PCBA** を第一候補にする。PCBA対象は公開BOM/CPLに含まれるダイオードとバッテリーコネクタだけに限定し、XIAO、スイッチ、ソケット、キーキャップ、電源スイッチ、バッテリー、ケース類は自分で購入・実装する。

PCBAで部品在庫、向き、費用に問題が出た場合だけ、PCBのみ発注に切り替える。

ただし、JLCPCBへ発注する前に次を確認する。

- `J8` を使うなら、Molex Pico-EZmate 1.2mm系に合うバッテリー/ケーブルを選び、極性を確認する
- XIAO nRF52840はdirect mount前提として扱い、ソケット化しない方針で組み立て手順を確認する
- 電源スイッチはGerber上の `SW23` footprintに合うthrough-hole SPDT slide switchを選ぶ

## 固定する製造ファイル

正本:

- Repository: <https://github.com/JonMuller/gerbers/tree/main/corne-choc-xiao>
- Commit: `53fe015c219462c6c3ef0841d53b17c03e8a4749`
- Commit date: 2025-06-21 UTC
- 設計名: `Corne-XIAO Reverso`

使うファイル:

| 用途 | ファイル | SHA-256 | メモ |
| ---- | ---- | ---- | ---- |
| PCB製造 | `GERBER-corne-choc-xiao.zip` | `af17960263833cc1e20b1368b8cdf4c39b5cf607cc2cac8135a3a71178e09e83` | JLCPCBに最初にアップロードする |
| PCBA部品表 | `BOM-corne-choc-xiao.csv` | `eebdda800166a5c882a9f32c51b345c6c670c96b8c82731cb557c70a78ca036b` | ダイオードとバッテリーコネクタのみ |
| PCBA配置 | `CPL-corne-choc-xiao.csv` | `50fbe5af51b798260b91fd6bb4b31446548a38a2180d161536dc8a3030f857bd` | 22点すべてbottom layer |

ファームウェア側の確認:

- `boards/shields/chipper/chipper.dtsi` は `diode-direction = "col2row";`
- JonMuller READMEのPCBA想定と矛盾しないため、diode directionは発注ブロッカーではない

メモ:

- 回路図上は1枚あたり21キー。左右は同じ `Reverso` 基板を反転して使う前提に見える
- 1台作るには最低2枚の使用可能なPCBが必要
- JLCPCBの最小発注枚数が2枚より多い場合は予備として扱う
- Gerber zip内に `corne-chocolate-VScore.gbr` が含まれるため、注文画面で追加料金やパネル扱いの警告が出ないか確認する
- 公開情報では、作者はJoy-Con系バッテリーを使い、XIAOを直接基板へマウントしている

## 発注方針

### 第一候補: PCB + 部分PCBA

理由:

- 手はんだでミスしやすいSMDダイオード21個/枚を減らせる
- バッテリーコネクタも小さいため、JLCPCB側で実装できるなら任せたい
- XIAOやスイッチ周りは自分で触るので、組み立て経験は残る

JLCPCBでやること:

1. `GERBER-corne-choc-xiao.zip` をアップロードする
2. PCB previewで外形、穴、左右用に使える枚数を確認する
3. PCB Assemblyを有効化する
4. `BOM-corne-choc-xiao.csv` と `CPL-corne-choc-xiao.csv` をアップロードする
5. 実装対象が `D1`-`D21` と `J8` だけになっていることを確認する
6. 部品配置previewで、ダイオードの向きと `J8` の向きを目視確認する
7. LCSC部品 `C2972761` と `C505023` の在庫、価格、代替提案を確認する
8. `C2972761` が JKSEMI `1N4148SOD-323`、SOD-323、single diodeとして認識されていることを確認する
9. `C505023` がMolex `78171-0002` 相当、Pico-EZmate 1.2mm pitch、2 contactsとして認識されていることを確認する
10. 問題がなければ、そのまま部分PCBAで発注する

### フォールバック: PCBのみ

次のどれかに当てはまる場合はPCBのみで発注する。

- JLCPCBの部品照合で `C2972761` または `C505023` が使えない
- PCBA追加費用が高すぎる
- 配置previewでダイオードやコネクタの向きに確信が持てない

PCBのみの場合の注意:

- ダイオードのマーカーはsquare through hole側へ向ける
- 逆向きに実装した場合はZMK側のmatrix direction調整が必要になる
- `J8` を使わず、代替のバッテリー接続パッドを使うかも発注前に決める

## レビュー反映

| 観点 | 状態 | 判断 |
| ---- | ---- | ---- |
| diode direction | 解消済み | `chipper.dtsi` が `col2row`。PCBA想定と一致する |
| `D1`-`D21` ダイオード | 解消済み | `C2972761` は JKSEMI `1N4148SOD-323`。BOM上の `Footprint=Diode-dual` は部品名ではなくfootprint名として扱う |
| `J8` バッテリーコネクタ | 発注前確認 | `C505023` はMolex `78171-0002` / Pico-EZmate 1.2mm / 2 contacts。一般的なJST-PH 2.0mm LiPoとは別物として扱う |
| XIAO direct mount | 発注前確認 | 公開情報ではdirect mount前提。ソケット化は初号機では採用しない方向で組み立て手順を確認する |
| リセットスイッチ | 追加購入しない | 公開BOM/CPLには独立したreset switchがない。XIAO側のreset操作で対応する |
| 電源スイッチ | 発注前確認 | `SW23` はGerber上でthrough-hole footprint。3 electrical pins + 2 mounting holesのslide switchとして照合する |

## 基板発注で来るもの

| 項目 | 数量 | 入手方法 | 状態 | メモ |
| ---- | ---- | ---- | ---- | ---- |
| `Corne-XIAO Reverso` PCB | 使用分2枚 | JLCPCB | 固定 | 余りは予備。左右は同一基板を反転して使う想定 |

## 実装サービスに任せるもの

公開BOM/CPL上の部品だけを対象にする。

| 項目 | Reference | 1枚あたり | 1台分 | LCSC | 状態 | メモ |
| ---- | ---- | ----: | ----: | ---- | ---- | ---- |
| 1N4148 SOD-323ダイオード | `D1`-`D21` | 21 | 42 | `C2972761` | PCBA第一候補 | JKSEMI `1N4148SOD-323`。matrix用。bottom layer |
| バッテリーコネクタ | `J8` | 1 | 2 | `C505023` | PCBA第一候補 | Molex `78171-0002` 相当。Pico-EZmate 1.2mm pitch、2 contacts |

## 自分で買うもの

| 項目 | 数量 | 状態 | メモ |
| ---- | ----: | ---- | ---- |
| Seeed Studio XIAO nRF52840 | 2 | 固定 | 無印。Plusではない。国内在庫と技適表記を確認して買う |
| Kailh Choc v1 Red | 42 + 予備 | 固定寄り | 50個買うと余裕がある |
| Choc v1 hot-swap socket | 42 + 予備 | 要確認 | PCB footprintに合うもの。50個買うと余裕がある |
| Choc標準キーキャップ | 42 | 要確認 | 18x17mm前提。19x19mm/MX spacing系は使わない |
| LiPoバッテリー | 2 | 未固定 | Joy-Con系/HAC-006候補。`J8` を使うならPico-EZmate 1.2mm側に合うケーブル/極性が必要 |
| 電源スイッチ | 2 | 要確認 | `SW23` に合うthrough-hole SPDT slide switch。JonMuller READMEのAmazonリンクか同等品から寸法を確認する |
| XIAO直付け用の消耗品 | 2セット | 方針確認 | ソケット化せずdirect mount前提。裏面バッテリーパッドへのジャンパー配線手順を確認する |
| ケース / ボトムプレート | 左右1セット | 後回し | まずは動作優先。必要ならJonMuller/LeducHのcase SVGを使う |
| ネジ / スペーサー / ゴム足 | 1セット | 後回し | ケース構成に合わせて決める |
| 予備ダイオード | 少量 | 任意 | PCBのみ発注へ切り替えた場合に必要。PCBAなら基本不要 |
| 予備バッテリーコネクタ | 少量 | 任意 | PCBのみ発注へ切り替えた場合に必要 |

買わないもの:

- XIAO nRF52840 Plus
- nice!nano
- OLED / nice!view
- RGB LED
- トラックボール部品
- エンコーダー
- TRRSジャック
- BLEドングル

## 発注前チェック

- `GERBER-corne-choc-xiao.zip` がJonMuller正本のものか確認する
- JLCPCBのpreviewで、1枚が21キーの片手基板として表示されるか確認する
- usable PCBが最低2枚ある注文になっているか確認する
- PCBA対象が `D1`-`D21` と `J8` だけか確認する
- `C2972761` が JKSEMI `1N4148SOD-323`、SOD-323、single diodeとして照合されるか確認する
- `D1`-`D21` の向きが公開READMEの注意と矛盾しないか確認する
- JLCPCBの部品配置previewでダイオードのpin 1/マーカー向きを確認する
- `J8` の向き、コネクタ型番、バッテリー側プラグ、極性を確認する
- JLCPCBが `J8` に代替部品を提案した場合、形状、ピッチ、極性が一致するまで採用しない
- バッテリー接続前に、届いた基板の `J8` とXIAO側バッテリー配線をテスターで確認する
- XIAOをdirect mountする前提で、裏面パッドへの配線手順をビルド写真で確認する
- XIAOをはんだ付けする前に、シルク、USB-C端子向き、左右反転時の表裏を照合する
- Choc標準18x17mmキーキャップを買う。19x19mmは避ける
- 電源スイッチは、購入前に `SW23` footprintと寸法を照合する

## 電源スイッチ `SW23` footprintメモ

Gerber上の `SW23` はpaste layerに出ておらず、solder maskとPTH drillに出ているため、through-hole実装のslide switchとして扱う。

確認できた穴配置:

| 用途 | 数 | Drill | Pitch / span | メモ |
| ---- | --: | ---- | ---- | ---- |
| electrical pins | 3 | 約0.8mm | 約2.0mm pitch | 中央3穴 |
| mounting holes | 2 | 約1.5mm | 外側間約8.2mm | 両端の固定穴 |

購入候補は、3 electrical pins + 2 mounting holesのSPDT slide switchに限定する。JonMuller READMEのAmazonリンクは参考にするが、商品ページが変わる可能性があるため、最終的には寸法で照合する。

## 参照

- JonMuller `corne-choc-xiao`: <https://github.com/JonMuller/gerbers/tree/main/corne-choc-xiao>
- LeducH整理版: <https://github.com/LeducH/corne-choc-xiao>
- Abomination v1.2: <https://kbd.news/Abomination-v1.2-1936.html>
- JonMuller build discussion: <https://www.reddit.com/r/ErgoMechKeyboards/comments/11ozztx/choc_spaced_corne_xiao/>
- JKSEMI `1N4148SOD-323`: <https://www.lcsc.com/product-detail/C2972761.html>
- Molex `78171-0002`: <https://www.newark.com/molex/78171-0002/wire-to-board-connector-pico-ezmate/dp/96W3687>
- Molex `781710002` / `C505023`: <https://www.lcsc.com/product-detail/C505023.html>
- JLCPCB BOM/CPL preparation: <https://jlcpcb.com/help/article/advice-for-bom-and-cpl-files-preparation>
- JLCPCB assembly file checklist: <https://jlcpcb.com/blog/files-needed-for-pcb-assembly>
