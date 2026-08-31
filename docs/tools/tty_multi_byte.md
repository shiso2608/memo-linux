# TTY でマルチバイト文字表示

KMSCON を使用して TTY 画面でマルチバイト文字を表示できるようにする。

## 目次

- [フォントをインストール](#フォントをインストール)
- [パッケージをインストール](#パッケージをインストール)
- [設定](#設定)
- [テスト](#テスト)
- [TTY を切り替え](#tty-を切り替え)

## フォントをインストール

```bash
sudo pacman -S --needed noto-fonts-cjk
```

## パッケージをインストール

```bash
sudo pacman -S --needed kmscon
```

## 設定

1. ファイル作成

```bash
sudo touch /etc/kmscon/kmscon.conf
```

2. 編集: `/etc/kmscon/kmscon.conf`

```diff
+ font-engine=pango
+ font-name=Noto Sans CJK JP
+ font-size=14
```

## テスト

1. tty へ移動: `左Ctrl + 左Alt + F2`
2. ログイン
3. 起動

```bash
sudo kmscon
```

4. ログイン
5. 確認

```bash
date
```

> [!TIP]
> 漢字が表示されれば良い。

6. 戻る: `左Ctrl + 左Alt + F1`

## TTY を切り替え

1. TTY 無効化

TTY2 を無効化する例:

```bash
sudo systemctl disable getty@tty2.service
```

2. KMSCON を有効化

KMSCON を TTY2 で有効化する例:

```bash
sudo systemctl enable kmsconvt@tty2.service
```

3. 再起動
4. 確認

```bash
systemctl status getty@tty2.service kmsconvt@tty2.service
```

> [!TIP]
> getty@tty2.service が **Active: inactive (dead)** 、kmsconvt@tty2.service が **Active: active (running)** と表示されれば良い。

5. 確認: `左Ctrl + 左Alt + F2`

> [!TIP]
> ログインした後に `date` を実行する。  
> 日付の漢字が表示されていれば良い。
