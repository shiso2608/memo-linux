# 初期設定

## 目次

- [パッケージマネージャーの高速化](#パッケージマネージャーの高速化)
- [システム全体のパッケージを更新](#システム全体のパッケージを更新)
- [RPM Fusion リポジトリを追加](#rpm-fusion-リポジトリを追加)
- [マルチメディアコーデックを追加](#マルチメディアコーデックを追加)
- [ハードウェアアクセラレーションを有効化](#ハードウェアアクセラレーションを有効化)
- [flathub の制限解除](#flathub-の制限解除)
- [セキュリティ](#セキュリティ)
- [スナップショット](#スナップショット)
- [ファームウェアの更新](#ファームウェアの更新)
- [システムログの容量制限](#システムログの容量制限)
- [ホーム下のディレクトリ名を英語に変更](#ホーム下のディレクトリ名を英語に変更)
- [参考](#参考)

## パッケージマネージャーの高速化

- 編集: `/etc/dnf/dnf.conf`

```diff
[main]
+ max_parallel_downloads=10
+ fastestmirror=True
```

## システム全体のパッケージを更新

```bash
sudo dnf --refresh upgrade
```

## RPM Fusion リポジトリを追加

公式リポジトリに含まれない、オープンソース以外のパッケージやメディアコーデック、サードパーティ製ドライバが含まれる。

1. 追加

```bash
sudo dnf install https://mirrors.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm https://mirrors.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm
```

2. 確認

```bash
dnf repolist
```

> [!TIP]
> **rpmfusion-free**, **rpmfusion-nonfree** が表示されれば良い。

## マルチメディアコーデックを追加

1. **ffmpeg** を非フリー版へ入れ替え

```bash
sudo dnf swap ffmpeg-free ffmpeg --allowerasing
```

2. マルチメディアパッケージをインストール

不要なパッケージを除外しつつ、必要なものだけインストールする。

```bash
sudo dnf install @multimedia --setopt="install_weak_deps=False" --exclude=PackageKit-gstreamer-plugin
```

## ハードウェアアクセラレーションを有効化

> [!CAUTION]
> 実機 (AMD GPU) 向けの設定。

- 64 bit

```bash
sudo dnf install mesa-va-drivers-freeworld
```

```bash
sudo dnf swap mesa-vulkan-drivers{,-freeworld}
```

- 32 bit

> [!CAUTION]
> Steam を使用する場合は必要になる。  
> しかしライブラリのバージョン問題を考えると、 Steam は flatpak 版を使用したほうが良い。

```bash
sudo dnf install mesa-va-drivers-freeworld.i686
```

```bash
sudo dnf swap mesa-vulkan-drivers{,-freeworld}.i686
```

## flathub の制限解除

1. 制限解除

```bash
flatpak remote-add --if-not-exists flathub https://dl.flathub.org/repo/flathub.flatpakrepo
```

2. 確認

```bash
flatpak remotes
```

> [!TIP]
> **flathub** に **filtered** が表示され無くなれば良い。

## セキュリティ

1. ファイアウォールを確認

```bash
sudo firewall-cmd --state
```

> [!TIP]
> **running** と表示されれば良い。

2. SELinux を確認

> [!NOTE]
> プロセスやファイルごとに詳細なアクセス権限を制限し、万が一アプリの脆弱性が突かれても被害をシステム全体に広げない強固なセキュリティ機能。  
> デフォルトの enforcing (強制モード) のまま運用することが推奨される。

```bash
sestatus
```

> [!TIP]
> **SELinux status: enabled**, **Current mode: enforcing** と表示されれば良い。

## スナップショット

> [!NOTE]
> システム全体や特定のファイルシステムの状態を特定の時点で保存する技術。  
> これにより、システムの状態を迅速かつ確実に復元することができる。

1. パッケージのインストール

```bash
sudo dnf install snapper
```

2. 設定ファイルを作成

```bash
sudo snapper -c root create-config /
```

3. スナップショットの自動作成を無効化

- 編集: `/etc/snapper/configs/root`

```diff
- TIMELINE_CREATE="yes"
+ TIMELINE_CREATE="no"
```

4. 初回作成

```bash
sudo snapper -c root create -d "初回スナップショット"
```

5. 確認

```bash
sudo snapper list
```

> [!TIP]
> スナップショットが作成されていれば良い。

## ファームウェアの更新

1. 確認

```bash
fwupdmgr get-updates
```

2. 更新

```bash
sudo fwupdmgr update
```

## システムログの容量制限

1. ディレクトリを作成

```bash
sudo mkdir -p /etc/systemd/journald.conf.d
```

2. ファイルを作成

```bash
sudo touch /etc/systemd/journald.conf.d/00-journal-size.conf
```

3. 編集: `/etc/systemd/journald.conf.d/00-journal-size.conf`

```conf
[Journal]
SystemMaxUse=500M
```

4. 設定を再読み込み

```bash
sudo systemctl daemon-reload
```

5. サービスを再起動

```bash
sudo systemctl restart systemd-journald
```

6. 確認

```bash
systemd-analyze cat-config systemd/journald.conf
```

> [!TIP]
> 表示内容の末尾に **SystemMaxUse=500M** が表示されれば良い。

## ホーム下のディレクトリ名を英語に変更

1. 変更

```bash
LANG=C xdg-user-dirs-update --force
```

> [!TIP]
> **Moving DESKTOP directory from デスクトップ to Desktop** 等が表示されれば良い。

2. 再起動
3. 標準フォルダーの名前を現在の言語に合わせて更新しますか?

- 次回から表示しない: **チェック**
- **古い名前のままにする**

> [!NOTE]
> ここより下の手順は、日本語のディレクトリ名が残っている場合に行う。

4. 日本語名のディレクトリの内容を移動

```bash
mv ~/デスクトップ/* ~/Desktop/.
```

> [!TIP]
> 全てのディレクトリで行う。

5. 日本語名のディレクトリを削除

```bash
trash-put ~/ダウンロード ~/テンプレート ~/デスクトップ ~/ドキュメント ~/ビデオ ~/音楽 ~/画像 ~/公開
```

6. 確認

```bash
ls -l
```

> [!TIP]
> 英語名のディレクトリだけ表示されれば良い。

## 参考

- [fedora](https://fedoraproject.org/)
- [RPM Fusion - Configuration](https://rpmfusion.org/Configuration)
- [RPM Fusion - Multimedia](https://rpmfusion.org/Howto/Multimedia?highlight=%28%5CbCategoryHowto%5Cb%29)
- [ArchWiki - systemd/Journal](https://wiki.archlinux.org/title/Systemd/Journal)
