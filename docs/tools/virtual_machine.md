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

fedora を例に進める。
