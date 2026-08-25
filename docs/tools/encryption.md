# ファイル暗号化 (age)

## 目次

- [パッケージインストール](#パッケージインストール)
- [鍵の生成](#鍵の生成)
- [ファイルの暗号化](#ファイルの暗号化)
- [ファイルの復号と閲覧](#ファイルの復号と閲覧)

## パッケージインストール

```bash
sudo pacman -S --needed age
```

## 鍵の生成

暗号化に使用する公開鍵と、復号に使用する秘密鍵を生成する。

1. 設定ディレクトリの作成

```bash
mkdir -p ~/.config/age
```

2. 鍵の生成

```bash
age-keygen -o ~/.config/age/key.txt
```

3. 公開鍵の抽出

生成した鍵から公開鍵を抽出する。

```bash
age-keygen -y ~/.config/age/key.txt > ~/.config/age/public_key.txt
```

## ファイルの暗号化

```bash
age -R ~/.config/age/public_key.txt -o confidential.age confidential.txt
```

> [!TIP]
> 暗号化が完了したら、元の生ファイルは安全に削除することを推奨する。

## ファイルの復号と閲覧

秘密鍵を使用して暗号化されたファイルを復号する。

- 内容の閲覧

```bash
age -d -i ~/.config/age/key.txt confidential.age | less
```

- ファイルの復号

```bash
age -d -i ~/.config/age/key.txt -o confidential.txt confidential.age
```
