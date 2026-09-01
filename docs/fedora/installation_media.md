# インストールメディアの作成

## 目次

- [手動で作成](#手動で作成)
  - [パッケージインストール](#パッケージインストール)
  - [ダウンロード](#ダウンロード)
  - [検証](#検証)
  - [書き込み](#書き込み)
- [Fedora Media Writer で作成](#fedora-media-writer-で作成)

## 手動で作成

ISO ファイルをダウンロードして、インストールメディアを作成する手順。

### パッケージインストール

```bash
sudo pacman -S --needed sequoia-sq
```

### ダウンロード

1. [fedora](https://fedoraproject.org/) にアクセス
2. **fedora WORKSTATION** の **Download Now** をクリック
3. **For Intel and AMD x86_64 systems** の 📥 **(Download)** をクリック  
  保存先は **~/Downloads**。
3. **For Intel and AMD x86_64 systems** の 📋️ **(Verify)** をクリック
4. **checksum file** をクリック  
  保存先は **~/Downloads**。
5. OpenPGP証明書 をダウンロード

```bash
cd ~/Downloads
```

```bash
curl -O https://fedoraproject.org/fedora.gpg
```

### 検証

1. 移動

```bash
cd ~/Downloads
```

2. 検証

```bash
gpgv --keyring ./fedora.gpg --output - \
    Fedora-Workstation-(バージョン番号)-x86_64-CHECKSUM \
    | sha256sum -c --ignore-missing
```

> [!TIP]
> **gpgv: "Fedora ((バージョン番号)) <fedora-(バージョン番号)-primary@fedoraproject.org>"からの正しい署名**,  
> **Fedora-Workstation-Live-(バージョン番号).x86_64.iso: 完了** と表示されれば良い。

### 書き込み

> [!CAUTION]
> 書き込み対象の USB メモリのデータはすべて消去される。  
> 指定するデバイス名 (`/dev/sdX` など) に誤りがないか必ず確認する。

1. USB メモリを接続
2. デバイス名を確認

```bash
lsblk
```

> [!TIP]
> この手順書内ではデバイス名を `/dev/sdX` とする

3. 移動

```bash
cd ~/Downloads
```

4. 書き込み

```bash
sudo dd if=Fedora-Workstation-Live-(バージョン番号).x86_64.iso of=/dev/sdX bs=8M status=progress oflag=direct
```

5. バッファ書き込み

```bash
sync
```

6. USB 取り外し

```bash
udisksctl power-off -b /dev/sda
```

## Fedora Media Writer で作成

Fedora Media Writer を使用して、インストールメディアを作成する手順。

> [!IMPORTANT]
> 自分の環境では動かなかった。  
> おそらく必要なパッケージが足りてない。 (諦)

### パッケージインストール

```bash
sudo pacman -S --needed mediawriter
```

### メディア作成

1. **Fedora Media Writer** を起動
2. イメージソースを選択する: **ダウンロードが自動的に行われます**
3. **次へ**
4. Fedora のリリースを選択してください

- 選択肢: **Official Editions**
- ドロップダウン: **Fedora Workstation**

5. **次へ**
6. 書き込みオプション

- バージョン: **(最新バージョン)**
- ハードウェアアーキテクチャ: **Intel/AMD 64bit**
- USBドライブ: **\(書き込む USB メモリ)**
- 書き込み後に削除する: **チェック**

7. **書き込み**
8. Erase confirmation: **書き込み**
