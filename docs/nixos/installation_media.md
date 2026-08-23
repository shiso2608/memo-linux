# NixOS インストールメディアの作成

Linux 環境で NixOS の USB インストールメディアを作成する手順です。

> [!Warning]
> `dd` コマンドを使用する際、出力先（`of=`）を間違えるとデータを失う恐れがあります。デバイス名の確認は慎重に行ってください。

## 目次

- [1. ISO イメージのダウンロード](#1-iso-イメージのダウンロード)
- [2. ファイルの検証](#2-ファイルの検証)
  - [整合性チェック (sha256)](#整合性チェック-sha256)
- [3. USB メモリへの書き込み](#3-usb-メモリへの書き込み)

---

## 1. ISO イメージのダウンロード

1. [NixOS 公式サイト](https://nixos.org/) の **Download** ページへアクセスします。
2. **NixOS : the Linux distribution** セクションの **ISO image** を開きます。
3. **Graphical ISO image** から、以下の 2 つのファイルをダウンロードし、同一ディレクトリ（例: `~/Downloads`）に保存します。
   - **Download (Graphical, 64-bit intel/AMD):** ISO イメージファイル `nixos-graphical-*-x86_64-linux.iso`
   - **(SHA-256):** ファイル検証用ファイル `nixos-graphical-*-x86_64-linux.iso.sha256`

## 2. ファイルの検証

ダウンロードしたディレクトリで作業を行います。

```bash
cd ~/Downloads
```

### 整合性チェック (sha256)

ダウンロードした ISO ファイルが破損していないか確認します。

```bash
sha256sum -c --ignore-missing nixos-graphical-*-x86_64-linux.iso.sha256
```

※ `nixos-graphical-*-x86_64-linux.iso: 完了` または `OK` と表示されれば正常です。

## 3. USB メモリへの書き込み

1. USB メモリを接続し、デバイス名を特定します。
   ```bash
   lsblk -f
   ```
   ※ `SIZE` や `FSTYPE` から対象のデバイス名（例: `/dev/sdX`）を特定してください。
2. `dd` コマンドで ISO を書き込みます（**要管理者権限**）。
   > [!Note]
   > `if=` には ISO ファイル名、`of=` には USB デバイス名を指定します。
   ```bash
   sudo dd bs=4M if=nixos-graphical-*-x86_64-linux.iso of=/dev/sdX conv=fsync oflag=direct status=progress
   ```
   - `bs=4M`: 読み書きのブロックサイズを4MBに設定。
   - `conv=fsync`: 完了前にデータを物理的に同期。
   - `oflag=direct`: キャッシュを介さず直接書き込み。
   - `status=progress`: 書き込み状況を表示。
3. キャッシュを完全に書き込みます。
   ```bash
   sync
   ```
4. 安全に USB メモリを取り外します。
   ```bash
   udisksctl unmount -b /dev/sdX1  # パーティションがマウントされている場合
   udisksctl power-off -b /dev/sdX
   ```

# 参考

- [NixOS Manual - Obtaining NixOS](https://nixos.org/manual/nixos/stable/#sec-obtaining)
- [NixOS Wiki - Create a USB Installation Media](https://nixos.wiki/wiki/Create_a_USB_Installation_Media)
