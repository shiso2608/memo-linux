# アンチウィルス (ClamAV)

## 目次

- [インストール](#インストール)
- [初期設定](#初期設定)
- [スキャンの実行例](#スキャンの実行例)

## インストール

1. パッケージインストール

```bash
sudo pacman -S --needed clamav
```

2. 確認

バージョンを確認して、インストールが完了しているかを確認する。

```bash
clamd --version
```

## 初期設定

1. 編集: /etc/clamav/clamd.conf

スキャンエンジンの設定を行う。
初めからコメントアウトされていたが、念の為確認する。

```diff
- Example
+ #Example
```

2. 編集: /etc/clamav/freshclam.conf

ウィルス定義更新の設定を行う。
初めからコメントアウトされていたが、念の為確認する。

```diff
- Example
+ #Example
```

3. ウィルス定義の手動更新
```bash
sudo freshclam
```

4. 自動更新サービスの有効化

```bash
sudo systemctl enable --now clamav-freshclam.service
```

5. スキャンエンジンの起動

```bash
sudo systemctl enable --now clamav-daemon.service
```

6. 確認

```bash
systemctl status clamav-daemon
```

## スキャンの実行例

- サービスとの通信確認

```bash
clamdscan --version
```

- 例: Downloads ディレクトリをマルチスレッドでスキャン

```bash
clamdscan --multiscan --fdpass ~/Downloads
```

> [!TIP]
> システムファイルなどの読み取り権限が必要な場所をスキャンする場合は、`sudo` を付加して実行する。
