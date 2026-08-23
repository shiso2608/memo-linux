# AUR ヘルパー (yay)

## インストール

1. パッケージインストール

ビルドに必要なパッケージをインストールする。

```bash
sudo pacman -S --needed base-devel git
```

2. 移動

```bash
cd /tmp
```

3. クローン

**yay** リポジトリをクローンする。

```bash
git clone https://aur.archlinux.org/yay.git
```

4. 移動

```bash
cd yay
```

5. インストール

パッケージをビルドして、インストールする。

```bash
makepkg -si
```

6. 確認

バージョンを表示して、インストールが完了したかを確認する。

```bash
yay --version
```

## 基本的な使い方

- システムの更新

```bash
yay -Syu
```

- パッケージの検索・インストール

```bash
yay -S <package_name>
```
