# シェル変更

標準の bash を zsh に変更する。

## 目次

- [パッケージをインストール](#パッケージをインストール)
- [設定](#設定)

## パッケージをインストール

1. インストール

- Arch Linux

```bash
sudo pacman -S --needed zsh
```

- fedora

```bash
sudo dnf install zsh
```

2. 再起動

## 設定

1. 場所を確認

```bash
command -v zsh
```

> [!TIP]
> **/usr/bin/zsh** が表示されれば良い。

2. 登録を確認

```bash
cat /etc/shells
```

> [!TIP]
> **/bin/zsh**, **/usr/bin/zsh** があれば良い。

3. シェルを変更

```bash
chsh -s $(command -v zsh)
```

> [!TIP]
> **Shell changed.** と表示されれば良い。

4. 再起動
