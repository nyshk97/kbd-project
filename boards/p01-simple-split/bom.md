# BOM

初号機の部品表。

発注・購入時の正本は [parts.md](./parts.md) とする。このファイルは概要だけを残す。

| 種別 | 部品 | 数量 | メモ |
| ---- | ---- | ---- | ---- |
| PCB | JonMuller `corne-choc-xiao` | 使用分2枚 | `GERBER-corne-choc-xiao.zip` をJLCPCBへアップロード |
| PCBA | 1N4148 SOD-323ダイオード / バッテリーコネクタ | 1台分 | BOM/CPLに含まれる `D1`-`D21` と `J8` だけ実装サービス候補 |
| MCU | Seeed Studio XIAO nRF52840 | 2 | 左右用。既存Corne Xiao系シールドとの互換性を優先 |
| スイッチ | Kailh Choc v1 Red | 42 + 予備 | 50個買うと余裕がある |
| キーキャップ | Choc標準18x17mm | 42 | 19x19mm/MX spacing系は避ける |
| ホットスワップソケット | Choc v1対応 | 42 + 予備 | footprint適合を確認 |
| バッテリー | 未定 | 2 | Joy-Con系/HAC-006候補。`J8` に合うケーブル/極性を確認 |
| 電源スイッチ | 未定 | 2 | `SW23` footprintに合うthrough-hole SPDT slide switch |
| リセットスイッチ | なし | 0 | p01では別体リセットスイッチを買わない想定 |
| XIAOソケット | なし | 0 | p01ではdirect mount前提 |
| ケース / プレート | 未定 | 左右1セット | 最初は簡易構成でよい |
| ネジ / スペーサー | 未定 | 1セット | ケースやプレート構成に合わせる |
