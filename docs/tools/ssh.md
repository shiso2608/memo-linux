# SSH 接続

## 目次

- [準備](#準備)
- [鍵の生成](#鍵の生成)
- [公開鍵を登録 (GitHub)](#公開鍵を登録-github)
- [SSH 鍵をキーリング管理](#ssh-鍵をキーリング管理)

## 準備

[キーリング](docs/tools/keyring.md) の設定を先に行う。

## 鍵の生成

```bash
ssh-keygen -t ed25519 -C "(任意のメールアドレス)"
```

> [!TIP]
> 鍵の生成時にパスフレーズの入力を求められる。  
> 入力せずに Enter キーを押すと、パスフレーズ無しで鍵を生成できる。

## 公開鍵を登録 (GitHub)

1. 確認: ~/.ssh/id_ed25519.pub

```bash
cat ~/.ssh/id_ed25519.pub
```

2. [GitHub](https://github.com/) へアクセス
3. SSH の登録画面へ移動  
  右上のアカウントアイコン -> Settings -> SSH and GPG keys
4. New SSH Key
5. 入力  
  - Title:  
    (任意のタイトル)
  - Key type:  
    Authentication Key
  - Key:  
    **~/.ssh/id_ed25519.pub** の内容をコピー & ペースト
6. Add SSH Key
7. 接続テスト

```bash
ssh -T git@github.com
```

> [!TIP]
> 「 **Hi (アカウント名)! You've successfully authenticated, but GitHub does not provide shell access.** 」 というメッセージが表示されていれば成功。

## SSH 鍵をキーリング管理

キーリングが解錠されている状態で SSH 接続を行った際に、パスフレーズの入力を省略できるようにする。

1.  SSH 鍵を追加

```bash
ssh-add ~/.ssh/id_ed25519
```

2. パスフレーズを入力
3. 確認

```bash
ssh-add -l
```

> [!TIP]
> 追加した SSH 鍵が表示されれば良い。
