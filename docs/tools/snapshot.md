# システムスナップショット (Snapper)

## 目次

- [パッケージインストール](#パッケージインストール)
- [初期設定](#初期設定)
- [手動スナップショットの作成](#手動スナップショットの作成)

## パッケージインストール

```bash
sudo pacman -S --needed snapper inotify-tools btrfs-assistant grub-btrfs
```

## 初期設定

1. 設定の作成

ルートディレクトリ (`/`) 用の Snapper 設定ファイルを作成する。

```bash
sudo snapper -c root create-config /
```

2. 自動クリーンアップの有効化

古いスナップショットを自動的に削除するタイマーを有効する。

```bash
sudo systemctl enable --now snapper-cleanup.timer
```

3. 保存世代数の設定

スナップショットの保持ポリシーを設定する。

```bash
sudo snapper -c root set-config \
  TIMELINE_LIMIT_HOURLY="0" \
  TIMELINE_LIMIT_DAILY="5" \
  TIMELINE_LIMIT_WEEKLY="0" \
  TIMELINE_LIMIT_MONTHLY="1" \
  TIMELINE_LIMIT_YEARLY="0" \
  NUMBER_LIMIT="10"
```

4. grub への統合

作成したスナップショットを起動時に選択できるように設定する。

```bash
sudo systemctl enable --now grub-btrfsd.service
```

```bash
sudo grub-mkconfig -o /boot/grub/grub.cfg
```

5. 確認

監視サービスが正しく動いているか確認する。

```bash
systemctl status grub-btrfsd.service
```

**Active: active (running)** と表示されていれば、以降はスナップショットが作成されるたびに grub メニューが自動更新される。

## 手動スナップショットの作成

大きな設定変更の前など、手動で状態を保存したい場合は以下を実行する。

```bash
sudo snapper -c root create --description "Manual Snapshot before major change"
```
