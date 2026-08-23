# インストール (archinstall)

## ブートメニュー

**Arch Linux installation media (x86_64, BIOS)** を選択する。

## インストーラー起動

```console
# pacman -Sy archinstall
# archinstall
```

## 設定内容

### Archinstall language

- Language: **English (100%)**

### Locales

- Keyboard layout: **us**  
  キーボード配列を設定する。日本語キーボードを使用する場合は jp106 などを選択する。
- Locale language: **en_US.UTF-8**  
  システムロケールの言語を設定する。文字化け対策のため、日本語はデスクトップ環境を整えたあとに設定する。
- Locale encoding: **UTF-8**  
  ロケールの文字エンコーディングを設定する。
- Console font: **default8x16**  
  仮想コンソール (TTY) で使用するフォントを設定する。

### Mirrors and repositories

- Select regions: **Japan**  
  パッケージのダウンロードに使用するミラーサーバーの地域を選択する。
- Add custom servers: **\(未設定)**  
  個別に手動でミラーサーバーの URL を追加する場合に使用する。
- Optional repositories: **multilib**  
  標準以外の追加リポジトリの有効 / 無効を切り替える。Steam や一部の 32ビットアプリを使う場合は multilib を有効にする。
- Add custom repository: **\(未設定)**  
  サードパーティ製などの独自リポジトリを追加する場合に使用する。

### Disk configuration

LVM, Disk encryption, Btrfs snapshots は Partitioning の設定が完了すると表示される。

- Partitioning
  1. Select a disk configuration: **Use a best-effort default partition layout**  
    パーティション構成方法を選択する。
  2. Select disks for the installation: **\(使用するストレージドライブ)**  
    OS をインストールする対象のドライブを選択する。
  3. Select main filesystem: **btrfs**  
    ルート領域などで使用するファイルシステムを選択する。
  4. Would you like to use BTRFS subvolumes with a default structure?: **Yes**  
    標準的な Btrfs サブボリューム構造を自動作成するかどうかを設定する。
  5. Would you like to use compression or disable CoW?: **Use compression**  
    透明なデータ圧縮を有効にしてストレージ容量を節約するか、または CoW を無効にするかを選択する。
- LVM: **\(未設定)**  
  複数のストレージをまとめて仮想的なボリュームを作成・管理する LVM を有効にするかを設定する。
- Disk encryption: **\(未設定)**  
  LUKS を使用してストレージ全体 (または特定領域) を暗号化する。
- Btrfs snapshots: **Snapper**  
  ファイルシステムに Btrfs を選択した場合に、スナップショット作成の自動構成を有効にするかを設定する。

### Swap

- Swap on zram: **Enabled**  
  RAM 上に圧縮スワップ領域を作成する zram 機能の有効/無効を設定する。ストレージへの書き込みを減らし、高速なスワップ処理が可能になる。
- Compression algorithm: **zstd**  
  zram で使用するデータ圧縮アルゴリズムを選択する。

### Bootloader

- Boot loader: **Grub**  
  OS を起動するために使用するブートローダーを選択する。
- Plymouth: **No**  
  見た目を良くするための起動画面を追加するが、一部の環境で起動の問題を引き起こす可能性がある。

### Kernels

- Kernel: **linux**  
  インストールするカーネルを選択する。

### Hostname

- Hostname: **\(任意のホスト名)**  
  PC の名前を設定する。

### Authentication

- Root password: **\(任意のパスワード)**  
  システム管理者 (root) アカウントのパスワードを設定する。
- User account
  日常的に使用する一般ユーザーアカウントを作成する。
  1. **Add a user**
  2. Enter a username: **\(任意のユーザー名)**  
    ユーザー名を設定する。
  3. Enter a password: **\(任意のパスワード)**  
    パスワードを設定する。
  4. Should "user" be a superuser (sudo)?: **Yes**  
    一般ユーザーに sudo 権限を持たせるか。
  5. **Confirm and exit**
- U2F login setup: **\(未設定)**  
  YubiKey などの U2F ハードウェアセキュリティキーを使用したログイン認証を設定する。

### Profile

- Profiles: **Minimal**  
  システムの用途に応じたプロファイルタイプを選択する。

### Applications

- Bluetooth: **Enabled**  
  Bluetooth の有効 / 無効を選択する。
- Audio: **pipewire**  
  システムで使用するオーディオサーバーを選択する。
- Print service: **Disabled**  
  プリンター印刷機能の有効 / 無効を選択する。
- Firewall: **ufw**  
  システムをネットワーク攻撃から保護するファイアウォール管理ツールを選択する。
- Additional fonts: **noto-fonts, noto-fonts-emoji, noto-fonts-cjk, ttf-liberation**  
  システムに追加でインストールするフォントパッケージを選択する。

### Network configuration

- Network configuration: **Use Network Manager (default backend)**  
  ネットワークの設定方式を選択する。

### Pacman

- Color: **True**  
  pacman コマンド実行時の出力テキストを色分けして視認性を高くするかを設定する。

### Additional packages

(任意のパッケージ)  
追加でインストールするパッケージを選択する。

### Timezone

- Timezone: **Japan**  
  タイムゾーンを選択する。

### Automatic time sync (NTP)

- NTP: **Enabled**  
  自動時刻同期の有効 / 無効を選択する。

## インストール

### Save configuration

設定した内容を保存する。

1. **Save all**
2. Enter a directory for the configuration(s) to be saved.: `/var/log/archinstall/`  
  保存先のディレクトリを入力する。
3. Do you want to save the configuration file(s) to /var/log/archinstall?: **Yes**
4. Do you want to encrypt the user_credentials.json file?: **No**  
  ユーザー設定ファイルを暗号化するか。

### Install

インストールを開始する。

1. The specified configuration will be applied. Would you like to continue?: **Yes**
2. What would you like to do next?: **Exit archinstall**  
  次の動作を選択する。
3. `cp /var/log/archinstall/* /mnt/var/log/archinstall/.`
4. `shutdown now`

### Abort

作業を中断する。
