## GPU ゼロ RPM

## 目次

- [パッケージインストール](#パッケージインストール)
- [サービスの有効化と起動](#サービスの有効化と起動)
- [ゼロ RPM 解除](#ゼロ-rpm-解除)

## パッケージインストール

```bash
pacman -Ss --needed lact
```

## サービスの有効化と起動

1. 有効化と起動

```bash
sudo systemctl enable --now lactd
```

2. 確認

```bash
systemctl status lactd
```

> [!TIP]
> **Active: active (running)** の表示があれば良い。

## ゼロ RPM 解除

1. **lact** を起動
2. 画面右上の **Enable AMD Overclocking**
3. **AMD Overclocking** 画面の **Enable AMD Overclocking**

> [!TIP]
> **Configuration updated, please reboot to apply changes.** と表示されれば良い。

4. PC 再起動
5. **lact** を起動
6. **Thermals** へ移動
7. **Zero RPM** のトグルスイッチを OFF に変更
8. 画面左下の **Apply**

> [!TIP]
> **Fan Speed** の数値が 0 でなくなれば良い。
