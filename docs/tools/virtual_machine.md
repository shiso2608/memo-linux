# 仮想マシン構築

## 目次

- [環境構築](#環境構築)
  - [パッケージインストール](#パッケージインストール)
  - [サポート確認](#サポート確認)
  - [ユーザー権限の設定](#ユーザー権限の設定)
  - [サービスの有効化](#サービスの有効化)
  - [仮想ネットワークの有効化と起動](#仮想ネットワークの有効化と起動)
- [仮想マシン作成](#仮想マシン作成)

## 環境構築

### パッケージインストール

```bash
sudo pacman -S --needed dnsmasq edk2-ovmf iptables-nft libvirt qemu-desktop virt-manager virt-viewer
```

### サポート確認

1. KVM モジュールのロードを確認

```bash
LC_ALL=C lscpu | grep Virtualization
```

> [!TIP]
> **AMD-V** と表示されれば良い。

2. モジュールの読み込みを確認

```bash
lsmod | grep kvm
```

> [!TIP]
> **kvm_amd** が表示されれば良い。

### ユーザー権限の設定

```bash
sudo usermod -aG libvirt $USER
```

### サービスの有効化

1. 有効化

```bash
sudo systemctl enable --now libvirtd.socket
```

2. 再起動

### 仮想ネットワークの有効化と起動

1. 有効化

```bash
sudo virsh net-autostart default
```

2. 起動

```bash
sudo virsh net-start default
```

3. 確認

```bash
sudo virsh net-list --all
```

> [!TIP]
> **状態: 動作中, 自動起動: はい (yes)** と表示されれば良い。

## 仮想マシン作成

1. サービス確認

```bash
systemctl status libvirtd
```

> [!TIP]
> 起動していなければ `sudo systemctl start libvirtd` で起動する。

2. **virt-manager** を起動
3. 画面左上の 🖥️ **(新しい仮想マシンの作成)** をクリック
4. オペレーティングシステムのインストール方法の選択: **ローカルのインストールメディア (ISO イメージまたは CD-ROM ドライブ)**
5. **次へ**
6. ISO または CDROM インストールメディアの選択: **参照** -> **(ISO ファイル)**
7. **次へ**
8. エミュレーターはパス `(ファイル名).iso` を検索する権限を持っていません。: **はい**
9. メモリと CPU の設定

- メモリ: **8192**
- CPU: **4**

10. **次へ**
11. 仮想マシン用にディスクイメージを作成する: **32** GiB
12. **次へ**
13. インストールを開始する準備ができました: **完了**
