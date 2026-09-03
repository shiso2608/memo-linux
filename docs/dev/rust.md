# Rust

## 目次

- [導入手順](#導入手順)
  - [標準的な手順](#標準的な手順)
  - [Arch Linux](#arch-linux)
- [コンポーネント追加](#コンポーネント追加)
- [リンカー追加](#リンカー追加)
- [確認](#確認)

## 導入手順

### 標準的な手順

1. インストール

```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

> [!TIP]
> **Current installation options:** はそのまま **Enter** を入力する。

> [!TIP]
> **Rust is installed now. Great!** と表示されればインストール完了。

2. パスを反映

```bash
source "$HOME/.cargo/env"
```

### Arch Linux

1. パッケージをインストール

```bash
sudo pacman -S --needed base-devel rustup
```

2. ツールチェーンを設定

```bash
rustup default stable
```

## コンポーネント追加

```bash
rustup component add rust-analyzer rust-src
```

## リンカー追加

C コンパイラを使用する。

- Arch Linux

```bash
sudo pacman -S --needed gcc
```

- fedora

```bash
sudo dnf install gcc
```

## 確認

```bash
rustc --version
```

```bash
cargo --version
```

```bash
gcc --version
```

> [!TIP]
> バージョン情報が表示されればインストール完了。
