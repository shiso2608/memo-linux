# 日本語環境構築

## 目次

- [日本語ロケール](#日本語ロケール)
- [日本語入力 (fcitx5)](#日本語入力-fcitx5)

## 日本語ロケール

1. 編集: **/etc/locale.gen**

```diff
- # ja_JP.UTF-8 UTF-8
+ ja_JP.UTF-8 UTF-8
```

2. ロケールを生成

```console
$ sudo locale-gen
Generating locales...
en_US.UTF-8... done
ja_JP.UTF-8... done
Generation complete.
```

3. 編集: **/etc/locale.conf**

```diff
- LANG=en_US.UTF-8
+ LANG=ja_JP.UTF-8
```

4. 再起動

```bash
sudo reboot
```

5. 確認

```console
$ locale
LANG=ja_JP.UTF-8
LC_CTYPE="ja_JP.UTF-8"
LC_NUMERIC="ja_JP.UTF-8"
LC_TIME="ja_JP.UTF-8"
LC_COLLATE="ja_JP.UTF-8"
LC_MONETARY="ja_JP.UTF-8"
LC_MESSAGE="ja_JP.UTF-8"
LC_PAPER="ja_JP.UTF-8"
LC_NAME="ja_JP.UTF-8"
LC_ADDRESS="ja_JP.UTF-8"
LC_TELEPHONE="ja_JP.UTF-8"
LC_MEASUREMENT="ja_JP.UTF-8"
LC_IDENTIFICATION="ja_JP.UTF-8"
LC_ALL=
```

## 日本語入力 (fcitx5)

1. パッケージインストール

```bash
sudo pacman -S --needed fcitx5 fcitx5-configtool fcitx5-gtk fcitx5-mozc fcitx5-qt
```

2. 編集: **~/.config/uwsm/env**

```diff
+ export GTK_IM_MODULE=fcitx
+ export QT_IM_MODULE=fcitx
+ export XMODIFIERS=@im=fcitx
```

3. **fcitx5-configtool** を起動
4. **現在の入力メソッド** に **Mozc** を追加
5. 再起動

```bash
reboot
```
