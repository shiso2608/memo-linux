# システム環境構築

## 目次

- [ハードウェア情報](#ハードウェア情報)
- [CPU マイクロコードとパフォーマンス設定](#cpu-マイクロコードとパフォーマンス設定)
- [GPU グラフィック設定](#gpu-グラフィック設定)
- [ストレージとシステム健全性](#ストレージとシステム健全性)
- [ファイアウォール](#ファイアウォール)

## ハードウェア情報

- CPU: **AMD Ryzen 7 5700X**
- GPU: **AMD Radeon RX 7900 XT**

## CPU マイクロコードとパフォーマンス設定

1. パッケージインストール

安定性の向上や脆弱性の対策を行う。

```bash
sudo pacman -S --needed amd-ucode
```

2. 編集: **/etc/default/grub**

省電力性とレスポンスの向上を行う。

```diff
- GRUB_CMDLINE_LINUX_DEFAULT="loglevel=3 quiet"
+ GRUB_CMDLINE_LINUX_DEFAULT="loglevel=3 quiet amd_pstate=active"
```

3. 更新

システムに反映する。

```bash
sudo grub-mkconfig -o /boot/grub/grub.cfg
```

## GPU グラフィック設定

1. パッケージインストール

GPU のグラフィック性能を発揮させる。

```bash
sudo pacman -S --needed mesa lib32-mesa vulkan-radeon lib32-vulkan-radeon
```

2. 編集: **/etc/mkinitcpio.conf**

ディスプレイ表示のちらつき防止や、画面出力の安定性を向上する。

```diff
- MODULES=()
+ MODULES=(amdgpu)
```

3. 更新

システムに反映する。

```bash
sudo mkinitcpio -P
```

## ストレージとシステム健全性

1. トリムタイマー

SSD のパフォーマンス低下を防ぎ寿命を延ばすため、タイマーを有効化する。

```bash
sudo systemctl enable fstrim.timer
```

2. スラブタイマー

データ整合性チェックのタイマーを有効化する。

```bash
sudo systemctl enable btrfs-scrub@-.timer
```

## ファイアウォール

1. 有効化

サービスを有効化する。

```bash
sudo systemctl enable --now ufw
```

2. 設定

設定をする。

```bash
sudo ufw default deny incoming
sudo ufw default allow outgoing
```

3. 開始

ファイアウォールを有効化する。

```bash
sudo ufw enable
```

## 再起動

```bash
sudo reboot
```
