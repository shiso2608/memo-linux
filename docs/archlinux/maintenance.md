# Arch Linux メンテナンス手順書

Arch Linux を健全な状態に保つための定期的なメンテナンス手順です。

## 目次

- [システム状態の確認](#システム状態の確認)
- [バックアップと準備](#バックアップと準備)
- [最新ニュースの確認](#最新ニュースの確認)
- [システムアップデート](#システムアップデート)
- [設定ファイルの管理 (.pacnew の処理)](#設定ファイルの管理-pacnew-の処理)
- [後処理とクリーンアップ](#後処理とクリーンアップ)

## システム状態の確認

アップデート前に現在のシステムに異常がないか確認します。

1. 失敗したサービスの確認

```bash
systemctl --failed
```

2. ログのエラー確認 (当日分)

```bash
journalctl -p err..alert --since="today"
```

## バックアップと準備

1. データのバックアップ

重要なデータのバックアップについては、[バックアップ手順書](docs/tools/backup.md) を参照してください。

2. スナップショットの作成 (Snapper 使用時)

システム変更前に復元ポイントを作成します。

```bash
sudo snapper -c root create --description "Before Update"
```

## 最新ニュースの確認

[Arch Linux 公式サイト](https://archlinux.org/) で、手動介入が必要なアップデートがないか確認してください。

## システムアップデート

1. ミラーリストの更新

```bash
sudo reflector --country 'Japan' --age 24 --protocol https --sort rate --save /etc/pacman.d/mirrorlist
```

2. パッケージの更新 (AUR を含む)

```bash
yay -Syu
```

## 設定ファイルの管理 (.pacnew の処理)

アップデートによって生成された新しい設定ファイルをマージします。

```bash
sudo DIFFPROG=meld pacdiff
```

**pacdiff 操作ガイド:**

- `v` (View): 差分を確認
- `s` (Skip): 後で対応するために一旦飛ばす
- `r` (Remove): `.pacnew` を削除（現在の設定を維持）
- `o` (Overwrite): `.pacnew` で上書き（新設定をそのまま採用）
- `m` (Merge): 既存の設定と新設定を結合（推奨）

**判断基準:**

|状況|推奨アクション|
|:---|:---|
|変更した記憶がないファイル|`o` (Overwrite)|
|パスワードや独自設定を含むファイル|`m` (Merge)|
|以前の仕様変更で不要になったファイル|`r` (Remove)|
|判断がつかない場合| `s` (Skip) して調査|

## 後処理とクリーンアップ

1. 再起動

カーネルやコアライブラリの更新を反映させるために再起動します。

```bash
reboot
```

2. パッケージキャッシュの削除

古いパッケージのキャッシュ（最新 3 バージョン以外）を削除します。

```bash
paccache -r
```

3. 未使用パッケージ (孤立したパッケージ) の削除

```bash
pacman -Qdtq | xargs -r sudo pacman -Rns
```

4. ディスク使用量の確認

```bash
gdu
```
