# Arch Linux インストールガイド

Btrfs + GRUB + AMD 環境向けのインストール手順です。

> [!Caution]
> **デュアルブート環境の場合**: 複数ストレージを利用している場合は、インストール対象以外のストレージを物理的に外すことを推奨します。
> **UEFI設定**: ホストマシンにインストールする場合、事前にセキュアブートを無効化してください。

## 目次

- [1. インストールメディアの起動](#1-インストールメディアの起動)
- [2. インストーラーの初期設定](#2-インストーラーの初期設定)
- [3. ストレージ構成](#3-ストレージ構成)
  - [パーティション作成](#パーティション作成)
  - [フォーマットとサブボリューム作成](#フォーマットとサブボリューム作成)
  - [マウント](#マウント)
- [4. 基本システムのインストール](#4-基本システムのインストール)
- [5. システムの設定](#5-システムの設定)
  - [chroot環境への移行](#chroot環境への移行)
  - [タイムゾーンとロケール](#タイムゾーンとロケール)
  - [ネットワークとコンソール](#ネットワークとコンソール)
  - [ユーザー管理](#ユーザー管理)
- [6. ブートローダーとドライバ](#6-ブートローダーとドライバ)
- [7. インストールの完了](#7-インストールの完了)

---

## 1. インストールメディアの起動

1. PCをシャットダウンし、インストールメディアを接続します。
2. PCを起動し、`Delete` キー（または `F2` 等）を連打して **UEFIメニュー** を表示します。
3. **起動 (Boot)** タブへ移動します。
4. **起動順位 #1** にインストールメディアを選択します。
5. **保存して終了** を選択し、再起動します。

## 2. インストーラーの初期設定

1. ブートメニューから **Arch Linux install medium (x86_64, UEFI)** を選択します。
2. コンソールのフォントサイズを見やすく変更します。
   ```console
   # setfont ter-120n
   ```
3. UEFIモード（64bit）で起動しているか確認します。
   ```console
   # cat /sys/firmware/efi/fw_platform_size
   ```
   ※ `64` と表示されれば正常です。
4. インターネット接続を確認します。
   ```console
   # ip link
   # ping -c 3 archlinux.jp
   ```
5. システムクロックを同期します。
   ```console
   # timedatectl status
   ```

## 3. ストレージ構成

### パーティション作成

1. ディスクの識別子を確認します（例: `/dev/sda`）。
   ```console
   # lsblk
   ```
2. `fdisk` でパーティションを設計します。
   ```console
   # fdisk /dev/sda
   ```
   - `g`: 新規GPTパーティションテーブルを作成
   - `n` -> `1` -> `Enter` -> `+1G`: EFIシステムパーティション
   - `n` -> `2` -> `Enter` -> `Enter`: ルートパーティション
   - `t` -> `1` -> `1`: パーティション1を "EFI System" に設定
   - `p`: 構成を確認
   - `w`: 書き込んで終了

### フォーマットとサブボリューム作成

1. EFIパーティションをフォーマットします。
   ```console
   # mkfs.fat -F 32 /dev/sda1
   ```
2. ルートパーティションを Btrfs で初期化し、サブボリュームを作成します。
   ```console
   # mkfs.btrfs /dev/sda2
   # mount /dev/sda2 /mnt
   # btrfs subvolume create /mnt/@
   # btrfs subvolume create /mnt/@home
   # btrfs subvolume create /mnt/@swap
   # btrfs subvolume create /mnt/@log
   # btrfs subvolume create /mnt/@pkg
   # umount /mnt
   ```

### マウント

1. マウントオプションを変数に定義します。
   ```console
   # export MOUNT_OPTIONS="noatime,compress=zstd,ssd,discard=async,space_cache=v2"
   ```
2. 各サブボリュームを適切なディレクトリにマウントします。
   ```console
   # mount -o subvol=@,$MOUNT_OPTIONS /dev/sda2 /mnt
   # mkdir -p /mnt/{boot,home,swap,var/log,var/cache/pacman/pkg}
   # mount -o subvol=@home,$MOUNT_OPTIONS /dev/sda2 /mnt/home
   # mount -o subvol=@log,$MOUNT_OPTIONS /dev/sda2 /mnt/var/log
   # mount -o subvol=@pkg,$MOUNT_OPTIONS /dev/sda2 /mnt/var/cache/pacman/pkg
   # mount -o subvol=@swap,noatime,ssd /dev/sda2 /mnt/swap
   # mount /dev/sda1 /mnt/boot
   ```

## 4. 基本システムのインストール

1. ミラーリストを最適化（日本国内のサーバーを優先）します。
   ```console
   # reflector --country 'Japan' --age 24 --protocol https --sort rate --save /etc/pacman.d/mirrorlist
   # pacman -Syy
   ```
2. 必須パッケージをインストールします。
   ```console
   # pacstrap -K /mnt base base-devel linux linux-firmware btrfs-progs os-prober networkmanager reflector openssh sudo helix git zsh
   ```
3. Fstabを生成します。
   ```console
   # genfstab -U /mnt >> /mnt/etc/fstab
   ```

## 5. システムの設定

### chroot環境への移行

インストールしたシステムにログインします。

```console
# arch-chroot /mnt
```

### タイムゾーンとロケール

1. 時間の設定。
   ```console
   # ln -sf /usr/share/zoneinfo/Asia/Tokyo /etc/localtime
   # hwclock --systohc
   ```
2. ロケールの生成。
   `/etc/locale.gen` を編集し、`en_US.UTF-8 UTF-8` と `ja_JP.UTF-8 UTF-8` のコメントアウトを解除します。
   ```console
   # helix /etc/locale.gen
   # locale-gen
   # echo "LANG=en_US.UTF-8" > /etc/locale.conf
   ```

### ネットワークとコンソール

1. ホスト名とネットワークマネージャーの設定。
   ```console
   # echo "your-hostname" > /etc/hostname
   # systemctl enable NetworkManager
   ```
2. キーボードとフォント（Terminus）の設定。
   ```console
   # pacman -S terminus-font
   # echo -e "KEYMAP=us\nFONT=ter-v22n" > /etc/vconsole.conf
   ```

### ユーザー管理

1. rootパスワードの設定。
   ```console
   # passwd
   ```
2. 一般ユーザーの作成と権限付与。
   ```console
   # useradd -m -G wheel (username)
   # passwd (username)
   # EDITOR=helix visudo
   ```
   ※ `%wheel ALL=(ALL:ALL) ALL` のコメントアウトを解除します。

## 6. ブートローダーとドライバ

1. GRUBのインストール。
   ```console
   # pacman -S grub efibootmgr grub-btrfs
   # grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=GRUB
   # grub-mkconfig -o /boot/grub/grub.cfg
   ```
2. AMD用マイクロコードとグラフィックドライバ。
   ```console
   # pacman -S amd-ucode mesa xf86-video-amdgpu vulkan-radeon
   # grub-mkconfig -o /boot/grub/grub.cfg
   ```

## 7. インストールの完了

1. 環境を抜けて再起動します。
   ```console
   # exit
   # umount -R /mnt
   # reboot
   ```
2. ログイン後、動作確認をして終了します。
   ```bash
   $ shutdown now
   ```
