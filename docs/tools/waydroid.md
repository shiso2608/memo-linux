# Android エミュレータ (waydroid)

## 目次

- [パッケージのインストール](#パッケージのインストール)
- [初期化](#初期化)
- [コンテナを起動](#コンテナを起動)
- [セッションの起動確認](#セッションの起動確認)
- [ARM 翻訳レイヤーの導入](#arm-翻訳レイヤーの導入)
- [Play ストアの認証](#play-ストアの認証)
- [参考](#参考)

## パッケージのインストール

- Fedora

```bash
sudo dnf install lzip waydroid
```

## 初期化

- OTA および GAPPS を有効化

```bash
sudo waydroid init -s GAPPS -c https://ota.waydro.id/system -v https://ota.waydro.id/vendor
```

## コンテナを起動

1. 起動

```bash
sudo systemctl enable --now waydroid-container
```

2. 確認

```bash
systemctl status waydroid-container
```

> [!TIP]
> **Active: active (running) ...** と表示されていれば良い。

## セッションの起動確認

1. 起動

```bash
waydroid session start
```

> [!TIP]
> **Android with user 0 is ready** が表示されると起動完了。

2. 画面を表示

別のターミナルを開いて、下記のコマンドを実行する。

```bash
waydroid show-full-ui
```

2. 停止

```bash
waydroid session stop
```

> [!TIP]
> 画面を閉じずにセッションを停止して大丈夫。

## ARM 翻訳レイヤーの導入

1. tmp へ移動

```bash
cd /tmp
```

2. waydroid_script プロジェクトをクローン

```bash
git clone https://github.com/casualsnek/waydroid_script.git
```

3. waydroid_script プロジェクトへ移動

```bash
cd waydroid_script
```

4. 仮想環境を作成

```bash
python3 -m venv .venv
```

5. 仮想環境を起動

```bash
source .venv/bin/activate
```

5. 必要なライブラリを導入

```bash
pip install -r requirements.txt
```

6. 翻訳レイヤーを導入

```bash
sudo .venv/bin/python main.py install libndk
```

> [!TIP]
> **libndk installation finished** が表示されれば成功。

7. 仮想環境を終了

```bash
deactivate
```

## Play ストアの認証

1. セッションを起動

```bash
waydroid session start
```

2. ID を取得

別のターミナルを開いて、下記のコマンドを実行する。

```bash
sudo waydroid shell -- sh -c "sqlite3 /data/data/*/*/gservices.db 'select value from main where name = \"android_id\";'"
```

> [!TIP]
> 表示された ID をコピーする。

3. ID を認証

- [デバイスの登録](https://www.google.com/android/uncertified) へ移動する。
- **Google サービスフレームワーク Android** 欄に ID をペーストする。
- **私はロボットではありません** にチェックを入れる。
- **登録** をクリックする。

> [!TIP]
> **デバイスを登録しました。** と表示されれば完了。

## 参考

- [Waydroid - Install Instructions](https://docs.waydro.id/usage/install-on-desktops)
- [Waydroid - Waydroid command line options](https://docs.waydro.id/usage/waydroid-command-line-options#init-options)
- [Waydroid - Google Play Certification](https://docs.waydro.id/faq/google-play-certification)
- [ArchWiki - Waydroid](https://wiki.archlinux.org/title/Waydroid)
- [waydroid_script](https://github.com/casualsnek/waydroid_script)
