# SSH 接続

## 目次

- [鍵の生成](#鍵の生成)
- [公開鍵を登録 (GitHub)](#公開鍵を登録-github)
- [エージェントを設定](#エージェントを設定)

## 鍵の生成

```bash
ssh-keygen -t ed25519 -C "(任意のメールアドレス)"
```

> [!TIP]
> 鍵の生成時にパスフレーズの入力を求められる。  
> 入力せずに Enter キーを押すと、パスフレーズ無しで鍵を生成できる。

## 公開鍵を登録 (GitHub)

1. 確認

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
    `~/.ssh/id_ed25519.pub` の内容をコピー & ペースト
6. Add SSH Key
7. 接続テスト

```bash
ssh -T git@github.com
```

> [!TIP]
> 「 **Hi (アカウント名)! You've successfully authenticated, but GitHub does not provide shell access.** 」 というメッセージが表示されていれば成功。

## エージェントを設定

1. サービスの起動と有効化

```bash
systemctl --user enable --now ssh-agent
```

2. 確認

```bash
systemctl --user status ssh-agent
```

> [!TIP]
> **Active: active (running)** と表示されれば良い。

3. 編集: `~/.config/uwsm/env`

```diff
+ export SSH_AUTH_SOCK="$XDG_RUNTIME_DIR/ssh-agent.socket"
```

4. ファイル作成

```bash
touch ~/.ssh/config
```

5. 編集: `~/.ssh/config`

```diff
+ Host *
+   AddKeysToAgent yes
```

6. 再起動
7. 確認

```bash
echo $SSH_AUTH_SOCK
```

> [!TIP]
> **/run/user/\<UID>/ssh-agent.socket** と出力されれば良い。

8. 接続テスト

```bash
ssh -T git@github.com
```

> [!TIP]
> 初回はパスフレーズの入力を求められる。  
> **Hi (アカウント名)!** と返ってくれば成功。

9. 登録確認

```bash
ssh-add -l
```

> [!TIP]
> ハッシュ値が表示されれば良い。
