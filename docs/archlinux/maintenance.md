# Arch Linux メンテナンス手順書

Arch Linux を健全な状態に保つための定期的なメンテナンス手順です。

## 1. システム状態の確認

アップデート前に現在のシステムに異常がないか確認します。

### 失敗したサービスの確認

```bash
systemctl --failed
```

### ログのエラー確認（当日分）

```bash
journalctl -p err..alert --since="today"
```

---

## 2. バックアップと準備

### データのバックアップ

重要なデータのバックアップについては、[バックアップ手順書](../Tools/Backup.md) を参照してください。

### スナップショットの作成 (Snapper 使用時)

システム変更前に復元ポイントを作成します。

```bash
sudo snapper -c root create --description "Before Update"
```

### 最新ニュースの確認

[Arch Linux 公式サイト](https://archlinux.org/) で、手動介入が必要なアップデートがないか確認してください。

---

## 3. システムアップデート

### ミラーリストの更新

```bash
sudo reflector --country 'Japan' --age 24 --protocol https --sort rate --save /etc/pacman.d/mirrorlist
```

### パッケージの更新 (AUR を含む)

```bash
yay -Syu
```

---

## 4. 設定ファイルの管理 (.pacnew の処理)

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

| 状況                                 | 推奨アクション      |
| :----------------------------------- | :------------------ |
| 変更した記憶がないファイル           | `o` (Overwrite)     |
| パスワードや独自設定を含むファイル   | `m` (Merge)         |
| 以前の仕様変更で不要になったファイル | `r` (Remove)        |
| 判断がつかない場合                   | `s` (Skip) して調査 |

---

## 5. 後処理とクリーンアップ

### 再起動

カーネルやコアライブラリの更新を反映させるために再起動します。

```bash
reboot
```

### パッケージキャッシュの削除

古いパッケージのキャッシュ（最新 3 バージョン以外）を削除します。

```bash
paccache -r
```

### 未使用パッケージ (孤立したパッケージ) の削除

```bash
pacman -Qdtq | xargs -r sudo pacman -Rns
```

### ディスク使用量の確認

```bash
gdu
```

### 重複ファイルの確認・削除

```bash
rmlint
# 作成されたスクリプトを実行して削除を確定
sh rmlint.sh
```
