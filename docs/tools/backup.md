# バックアップ (rclone)

## 目次

- [パッケージインストール](#パッケージインストール)
- [バックアップ対象設定](#バックアップ対象設定)
- [バックアップ実行](#バックアップ実行)
- [エイリアス (zsh)](#エイリアス-zsh)

## パッケージインストール

```bash
sudo pacman -S --needed rclone
```

## バックアップ対象設定

1. ディレクトリ作成

```bash
mkdir -p ~/.config/rclone
```

2. ファイル作成

```bash
touch ~/.config/rclone/backup_list
```

3. 編集: ~/.config/rclone/backup_list

下記は例。

```
.config/**
Desktop/**
Documents/**
Music/**
Pictures/**
Videos/**
```

## バックアップ実行

```bash
rclone sync ~ (任意のバックアップ先) --include-from ~/.config/rclone/backup_list --progress
```

## エイリアス (zsh)

- 編集: ~/.zshrc

```diff
+ alias backup-run='rclone sync ~ (任意のバックアップ先) --include-from ~/.config/rclone/backup_list --progress'
```
