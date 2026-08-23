# Arch Linux インストールメディアの作成

Linux 環境で Arch Linux の USB インストールメディアを作成する手順です。

> [!Warning]
> `dd` コマンドを使用する際、出力先（`of=`）を間違えるとデータを失う恐れがあります。デバイス名の確認は慎重に行ってください。

## 目次

- [1. ISO イメージのダウンロード](#1-iso-イメージのダウンロード)
- [2. ファイルの検証](#2-ファイルの検証)
  - [整合性チェック (b2sum)](#整合性チェック-b2sum)
  - [署名チェック (GnuPG)](#署名チェック-gnupg)
- [3. USB メモリへの書き込み](#3-usb-メモリへの書き込み)

---

## 1. ISO イメージのダウンロード

1. [Arch Linux 公式サイト](https://archlinux.org/) の **Download** ページへアクセスします。
2. ページ上部の **PGP fingerprint** のリンクを、後の署名検証のために開いておきます。
3. 日本国内のミラーサイトから、以下の3つのファイルをダウンロードし、同一ディレクトリ（例: `~/Downloads`）に保存します。
   - `archlinux-YYYY.MM.DD-x86_64.iso`
   - `archlinux-YYYY.MM.DD-x86_64.iso.sig`
   - `b2sums.txt`

## 2. ファイルの検証

ダウンロードしたディレクトリで作業を行います。

```bash
cd ~/Downloads
```

### 整合性チェック (b2sum)

ダウンロードした ISO ファイルが破損していないか確認します。

```bash
b2sum -c --ignore-missing b2sums.txt
```

※ `archlinux-YYYY.MM.DD-x86_64.iso: OK` と表示されれば正常です。

### 署名チェック (GnuPG)

1. 署名キーを取得します。
   ```bash
   gpg --auto-key-locate clear,wkd -v --locate-external-key pierre@archlinux.org
   ```
2. 出力された `fingerprint` が、手順1で確認した公式サイトの指紋と一致することを確認します。
3. 署名を検証します。
   ```bash
   gpg --verify archlinux-YYYY.MM.DD-x86_64.iso.sig archlinux-YYYY.MM.DD-x86_64.iso
   ```
   ※ `完結した署名 (Good signature)` と表示されれば信頼できます。

## 3. USB メモリへの書き込み

1. USB メモリを接続し、デバイス名を特定します。
   ```bash
   lsblk
   ```
   ※ `SIZE` や `TYPE` から対象のデバイス名（例: `/dev/sdX`）を特定してください。
2. `dd` コマンドで ISO を書き込みます（**要管理者権限**）。
   > [!Note]
   > `if=` には ISO ファイル名、`of=` には USB デバイス名を指定します。
   ```bash
   sudo dd bs=4M if=archlinux-YYYY.MM.DD-x86_64.iso of=/dev/sdX conv=fsync oflag=direct status=progress
   ```
3. キャッシュを完全に書き込みます。
   ```bash
   sync
   ```
4. 安全に USB メモリを取り外します。
   ```bash
   udisksctl unmount -b /dev/sdX1  # パーティションがマウントされている場合
   udisksctl power-off -b /dev/sdX
   ```
