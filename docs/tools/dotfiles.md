# dotfiles 管理

## 目次

- [インストール](#インストール)
- [初期設定](#初期設定)
- [基本的な操作](#基本的な操作)
  - [追加](#追加)
  - [編集](#編集)
  - [変更の適用](#変更の適用)
- [パッケージの一括管理](#パッケージの一括管理)
- [リモートリポジトリとの同期](#リモートリポジトリとの同期)
  - [初回のプッシュ](#初回のプッシュ)
  - [継続的な運用](#継続的な運用)
  - [別の環境で復元する場合](#別の環境で復元する場合)

## インストール

1. パッケージインストール

```bash
sudo pacman -S --needed chezmoi git
```

2. 確認

バージョンを表示して、インストールが完了したかを確認する。

```bash
chezmoi --version
git --version
```

## 初期設定

ローカルリポジトリを作成する。
管理用のディレクトリ（デフォルトは `~/.local/share/chezmoi`）を初期化する。

```bash
chezmoi init
```

## 基本的な操作

### 追加

既存の設定ファイルを管理下にコピーする。

```bash
chezmoi add ~/.zshrc
```

### 編集

管理下のファイルをエディタで直接編集する。

```bash
chezmoi edit ~/.zshrc
```

## 変更を適用

管理下のファイルと実際の設定ファイルの差分を確認し、変更を反映する。

```bash
# 差分の確認
chezmoi diff
```

```bash
# 反映（-v で詳細表示）
chezmoi apply -v
```

## パッケージの一括管理

テンプレート機能を利用して、OS セットアップ時に必要なパッケージを自動インストールする設定です。

1. パッケージリストを定義

管理ディレクトリへ移動し、定義ファイルを作成する。

```bash
chezmoi cd
mkdir .chezmoidata
touch .chezmoidata/packages.yaml
```

2. 編集: **.chezmoidata/packages.yaml**

```yaml:.chezmoidata/packages.yaml
packages:
  pacman:
    - ghostty
    - neovim
    - helix
    - zsh
    - git
  yay:
    - google-chrome
```

3. インストールスクリプトの作成

ファイル名に `run_onchange_` を含めることで、管理下のファイルに変更があった際に自動で実行される。

```bash
# 管理ディレクトリ内で作成
touch run_onchange_install-packages.sh.tmpl
chmod +x run_onchange_install-packages.sh.tmpl
```

4. 編集: run_onchange_install-packages.sh.tmpl

```sh:run_onchange_install-packages.sh.tmpl
{{ if eq .chezmoi.os "linux" -}}
#!/bin/bash

# pacman パッケージのインストール
sudo pacman -S --needed --noconfirm {{ .packages.pacman | join " " }}

# yay が存在する場合のみ実行
if command -v yay >/dev/null 2>&1; then
    yay -S --needed --noconfirm {{ .packages.yay | join " " }}
fi
{{ end -}}
```

```bash
# chezmoi のシェルから抜ける
exit
```

## リモートリポジトリとの同期

管理ディレクトリは Git リポジトリになっているため、GitHub 等でバックアップ・共有が可能。

### 初回のプッシュ

```bash
chezmoi cd
git add .
git commit -m "Initial commit: setup dotfiles and package management"
git remote add origin <リポジトリURL>
git push -u origin main
```

### 継続的な運用

1. **設定の追加/変更**: `chezmoi add` または `chezmoi edit`
2. **反映**: `chezmoi apply -v`
3. **保存**: `chezmoi cd` して `git commit` & `git push`

### 別の環境で復元する場合

```bash
chezmoi init --apply <リポジトリURL>
```
