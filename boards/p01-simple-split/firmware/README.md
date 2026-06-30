# firmware

p01-simple-split用のZMK config作業場所。

## 現在の状態

- `zmk-config/` は `wintinue/zmk-corne` をベースにしたコピー
- 取り込み元commit: `0236c3a9f466411806be85f202e3c509db495592`
- 取り込み元commit日付: 2026-05-16
- `zmk-config/build.yaml` のboard名を `xiao_ble//zmk` に更新済み
- root workflow `.github/workflows/p01-zmk-build.yml` で `chipper_left` / `chipper_right` のUF2をビルドする
- GitHub Actions run `28447236060` で左右のUF2生成を確認済み

## 次にやること

- 実機入手後、生成済みUF2を左右のXIAO nRF52840へ書き込んで起動確認する
