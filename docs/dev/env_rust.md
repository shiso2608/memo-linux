# Rust の導入手順

Arch Linux に `rustup` を使用して Rust 開発環境を導入する手順です。

## 1. インストール

パッケージマネージャーから `rustup` と、デバッグに必要な `lldb` をインストールします。

```bash
sudo pacman -S rustup lldb
```

## 2. ツールチェーンの設定

インストール後、デフォルトのツールチェーンを `stable` に設定します。

```bash
rustup default stable
```

## 3. コンポーネントの追加

開発を快適にするために、エディタ支援に必要なコンポーネントを追加します。

```bash
rustup component add rust-analyzer rust-src
```

- **rust-analyzer**: 高機能な言語サーバー (LSP)。
- **rust-src**: 標準ライブラリのソースコード（補完や定義ジャンプに使用）。

## 4. 動作確認

正しくインストールされたか、バージョンを確認します。

```console
$ cargo --version
cargo 1.95.0 (f2d3ce0bd 2026-03-21)
```