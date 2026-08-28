# Rust

## 目次

- [パッケージインストール](#パッケージインストール)
- [設定](#設定)

## パッケージインストール

```bash
sudo pacman -S --needed base-devel rustup
```

## 設定

1. ツールチェーンの設定

```bash
rustup default stable
```

2. コンポーネントの追加

```bash
rustup component add rust-analyzer rust-src
```

3. 確認

```bash
rustc --version
```

```bash
cargo --version
```
