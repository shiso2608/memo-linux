# メール (Thunderbird)

## 目次

- [パッケージインストール](#パッケージインストール)
- [ブリッジ設定](#ブリッジ設定)
- [ブリッジ起動](#ブリッジ起動)
- [メーラー設定 (Thunderbird)](#メーラー設定-thunderbird)
- [メッセージフィルター](#メッセージフィルター)

## パッケージインストール

```bash
sudo pacman -S --needed protonmail-bridge-core thunderbird thunderbird-i18n-ja
```

## ブリッジ設定

1. 起動

```bash
protonmail-bridge-core --cli
```

2. ログイン

proton のメールアカウントにログインする。

```bash
login
```

- Username: **(アカウントのメールアドレス、もしくはユーザー名)**
- Password: **(アカウントのパスワード)**
- Two factor code: **(2 段階認証のコード)**

3. 確認

ブリッジへの接続情報を確認する。

```bash
info
```

> [!TIP]
> 表示された内容はメーラーの設定に使用するため、メモ等をして確認できるようにしておく。

4. 終了

```bash
exit
```

## ブリッジ起動

1. サービスの起動と有効化

```bash
systemctl --user enable --now protonmail-bridge.service
```

2. 確認

```bash
systemctl --user status protonmail-bridge.service
```

> [!TIP]
> **Active: active (running)** の表示があれば良い。

## メーラー設定 (Thunderbird)

1. メールアドレスの追加

- 氏名: **(任意の氏名)**
- メールアドレス: **(アカウントのメールアドレス)**

2. **手動設定**
3. 受信サーバー設定

> [!TIP]
> `info` を実行した際の **IMAP Settings** の内容を参照する。

- プロトコル: **IMAP**
- ホスト名: **(Address)**
- ポート番号: **(IMAP port)**
- 接続の保護: **STARTTLS**
- 認証方式: **通常のパスワード認証**
- ユーザー名: **(Username)**

4. 送信サーバー設定

> [!TIP]
> `info` を実行した際の **SMTP Settings** の内容を参照する。

- プロトコル: **SMTP**
- ホスト名: **(Address)**
- ポート番号: **(SMTP port)**
- 接続の保護: **STARTTLS**
- 認証方式: **通常のパスワード認証**
- ユーザー名: **(Username)**

5. **テスト**
6. パスワード: **(ブリッジへの接続情報の Password)**
7. Thunderbird が例外的に信頼する証明書としてこのサイトの証明書を登録しようとしています。: **セキュリティ例外を承認**
8. **完了**
9. Thunderbird を次の既定のクライアントとして使用する: **既定として設定**

## メッセージフィルター

- フィルター名: **自動削除**
- フィルターを適用するタイミング:  
  **手動で実行する, 新着メール受信時 (迷惑メール分類前に実行), 定期的、10 分ごと**
- すべての条件に一致
  - **送信日からの日数, が次の値を超える, 30**
  - **状態, が次と異なる, スター付き**
- 以下の動作を実行する:  
  **メッセージを移動する, ゴミ箱(メールアドレス)**
