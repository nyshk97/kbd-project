# p01-simple-split

ZMK、XIAO nRF52840、Choc v1、左右分割、親指キーの基本フローを掴むための初号機。

## 確定仕様

- 無線分割
- ZMK
- Corne Xiao系42キー
- Kailh Choc v1
- Seeed Studio XIAO nRF52840
- トラックボールなし
- エンコーダーなし
- BLEドングルなし
- XIAO nRF52840 Plusなし

## 目的

- ZMKのビルド、書き込み、左右分割設定に慣れる
- XIAO nRF52840前提のZMK構成と配線に慣れる
- Choc v1の配列、キースペース、親指キー位置を体感する
- レイヤー運用で数字、記号、矢印を扱えるか確認する
- PCB、ケース、ファームウェア、組み立ての一連の流れを短く一周する

## スコープ

やること:

- 左右分割
- 42キー前後
- 親指キー
- ZMK
- Seeed Studio XIAO nRF52840
- Choc v1

やらないこと:

- BLEドングル
- PMW3610トラックボール
- EVQWGD001エンコーダー
- XIAO nRF52840 Plusの追加GPIO検証
- OLED / RGB
- 独自キーキャップ
- 複雑な立体ケース

## 完了条件

- 全キーが入力できる
- 左右分割通信が安定する
- XIAO nRF52840でZMKのビルド、書き込み、ペアリングができる
- 最低限のレイヤーで日常入力を試せる
- 親指キー位置の良し悪しを記録できる

## 選定固定

部品購入やPCB発注の前に、使うCorne Xiao系設計を1つに固定する。

確認するもの:

- リポジトリURLとcommit
- Gerberの有無
- BOM
- Choc v1対応か
- XIAO nRF52840対応か
- 左右の基板がリバーシブルか、左右別か
- ZMK shield定義の入手方法
- `build.yaml` のboard/shield例
- ZMK Studio対応の有無
- バッテリー端子、電源スイッチ、リセットスイッチの扱い

注意:

- 無印CorneはPro Micro前提なので、Corne Xiao系と取り違えない
- `Corne Xiao` という公式統一shield名があるわけではなく、作者ごとにshield名が違う
- ZMK公式のcurrent targetではXIAO nRF52840のboardは原則 `xiao_ble//zmk`
- shield名は選定した基板repoの `boards/shields/` 定義に従う
- 外部shieldの場合はzmk-config内にshield定義を置くか、ZMK moduleとして取り込む

## 実行手順

1. Corne Xiao系設計を選定固定する
2. 基板の入手方法を決める。完成品を買うか、GerberからJLCPCB等へ発注する
3. BOMを基板側の部品表で更新し、不足部品を購入する
4. 工具を準備する。温度調整付きはんだごて、はんだ、フラックス、テスター、ピンセットを前提にする
5. ダイオード、ホットスワップソケット、スイッチ、XIAO、電源まわりの順に組み立てる
6. 各キーと電源まわりの導通をテスターで確認する
7. ZMK configを作る。ZMK CLIまたは基板repoのサンプルconfigを起点にする
8. `build.yaml` に左右ターゲットを追加する。boardは原則 `xiao_ble//zmk`、shieldは基板repoの定義名を使う
9. 最小keymapを書く
10. GitHub Actionsでビルドし、左右の`.uf2`を取得する
11. XIAOをブートローダーモードにして左右それぞれへ`.uf2`を書き込む
12. 左右を同時リセットし、split接続を確認する
13. PCとはcentral側、通常は左手だけをBluetoothペアリングする
14. 全キー入力テストを行う
15. keymapを編集、push、再ビルド、再フラッシュして反復する

## ZMKメモ

- 古い記事やサンプルでは `seeeduino_xiao_ble` など旧board名が出ることがある
- current ZMKのXIAO nRF52840 board targetは `xiao_ble//zmk` を前提に確認する
- 実際に使うboard/shield名は、選定したCorne Xiao系repoと現在のZMK公式ドキュメントを優先する
- 分割構成ではcentral側がPCへHID出力する。peripheral側はPCと直接ペアリングしない
