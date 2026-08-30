# デュアルブート

GRUB メニューから Windows を起動できるようにする。

## 目次

- [パッケージインストール](#パッケージインストール)
- [設定](#設定)

## パッケージインストール

```bash
sudo pacman -S --needed ntfs-3g os-prober
```

## 設定

1. 編集: `/etc/default/grub`

```diff
- #GRUB_DISABLE_OS_PROBER=false
+ GRUB_DISABLE_OS_PROBER=false
```

2. 更新

```bash
sudo grub-mkconfig -o /boot/grub/grub.cfg
```

> [!TIP]
>  **Found Windows Boot Manager on /dev/(ドライブ名)/efi/Microsoft/Boot/bootmgfw.efi** と表示されれば良い。
