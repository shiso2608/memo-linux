# 環境を再構築

## 目次

- [設定リポジトリのコピー](#設定リポジトリのコピー)
- [ハードウェア設定の入れ替え](#ハードウェア設定の入れ替え)
- [ビルド](#ビルド)

## 設定リポジトリのコピー

1. シェル環境を用意する

```bash
nix-shell -p git
```

2. ホームディレクトリに移動する

```bash
cd
```

3. 設定リポジトリをコピーする

```bash
git clone https://github.com/shiso2608/nixos-config.git
```

## ハードウェア設定の入れ替え

1. 既存設定をバックアップする

```bash
mv ~/nixos-config/hardware/hardware-configuration.nix ~/nixos-config/hardware/hardware-configuration.nix.backup
```

2. 新規設定をコピーする

```bash
cp /etc/nixos/hardware-configuration.nix ~/nixos-config/hardware/.
```

## ビルド

1. ディレクトリを移動する

```bash
cd ~/nixos-config
```

2. ステージングする

```bash
git add hardware/hardware-configuration.nix
```

3. ビルドする

```bash
sudo nixos-rebuild boot --flake .
```

4. シェル環境から抜ける

```bash
exit
```

5. 再起動する

```bash
sudo reboot
```
