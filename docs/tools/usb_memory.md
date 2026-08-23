# USB メモリの操作

## 目次

- [マウント](#マウント)
- [アンマウント](#アンマウント)

## マウント

1. デバイスの特定

```bash
lsblk
```

- **SIZE**: 容量から判断する。
- **MOUNTPOINTS**: 空欄のものがマウント前のデバイス。
- **NAME**: 通常 `/dev/sdb1` や `/dev/sdc1` のようになる。

2. マウント

例として **/dev/sdx1** をマウントする場合。

```bash
udisksctl mount -b /dev/sdx1
```

3. マウント先への移動

```bash
cd /run/media/$USER/ラベル名
```

## アンマウント

1. データの同期

```bash
sync
```

2. アンマウント

```bash
udisksctl unmount -b /dev/sdb1
```

> [!TIP]
> **target is busy** と表示される場合は、そのディレクトリを開いているシェルやアプリがないか確認し、`cd` などで別のディレクトリに移動してください。

3. 電源供給の停止

```bash
udisksctl power-off -b /dev/sdb
```

4. 確認と物理的な取り外し

再度 `lsblk` を実行し、該当するデバイス名が表示されなくなっていれば、物理的に引き抜いても安全。
