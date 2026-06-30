# Build Log

組み立て、失敗、変更、気づきを時系列で記録する。

## ログ

- 2026-06-30: `firmware/zmk-config/` に `wintinue/zmk-corne` を取り込み。取り込み元commitは `0236c3a9f466411806be85f202e3c509db495592`。
- 2026-06-30: `firmware/zmk-config/build.yaml` のboard名を `seeeduino_xiao_ble` から `xiao_ble//zmk` に更新。
- 2026-06-30: p01用ZMK configをGitHub Actionsでビルドするroot workflow `.github/workflows/p01-zmk-build.yml` を追加。
- 2026-06-30: GitHub Actions初回実行は、ZMKの再利用workflowがネストした `zmk-config` 配置を前提にしておらず `west update` で失敗。p01専用workflowに差し替え、左右のUF2だけをビルドする構成に変更。
