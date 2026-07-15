# kbd-project

設計判断は `docs/decisions.md`、発注記録は `boards/*/orders.md` を参照。

## JLCPCB / JLC3DP 発注の癖

- **JLCPCBのBOM画面は、部品がマッチ済みでも実装対象のSelectチェックが外れていることがある**（Qtyが0のまま）。実装したい行のチェックONを必ず確認。逆に実装しない部品（ソケット等）はチェックOFFのまま「Do not place」で進む
- **JLC3DPはReview Before Payment方式**: Submit Order→ファイルレビュー（10分〜4時間）→承認後に支払い。自動決済はオフにして、レビューでの問い合わせ（コンスルー穴→「そのまま製造」等）を確認してから手動で払う
- **JLC3DPのSLAレジン色**: LEDO 6060はNatural White一色のみ。黒が欲しい場合はJLC Black Resinが最安かつ高強度・高靭性（Black Resinは灰味の"Grayish Black"、Imagine Blackは外観検証向けで割高）
- **注文フォームのProduct Desc / Product Descriptionは必須**（未選択だとSave/注文がエラー）。キーボード類は「Office Appliance and Accessories → (Plastic) Keyboard (Enclosure)」
- 部品配置プレビューが枠外に飛んだらツールバーの画面フィットボタン（リロード不要、自動保存あり）。Top/Bottom切り替えで表が「No Data」なのは実装面違いを見ているだけ
