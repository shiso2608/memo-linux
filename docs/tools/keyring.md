# キーリング

## 目次

- [パッケージインストール](#パッケージインストール)
- [設定](#設定)
- [SSH 鍵を管理](#ssh-鍵を管理)
- [PAM 連携](#pam-連携)

## パッケージインストール

```bash
sudo pacman -S --needed gnome-keyring libsecret
```

## 設定

1. 編集: ~/.config/uwsm/env

```diff
+ export SECRET_SERVICE_BUS_NAME=org.freedesktop.secrets
```

## SSH 鍵を管理

1. サービスの起動と有効化

```bash
systemctl --user enable --now gcr-ssh-agent.socket
```

2. ソケットファイルの確認

```bash
ls -l $XDG_RUNTIME_DIR/gcr/ssh
```

## PAMの連携

1. 編集: /etc/pam.d/greetd

```diff
#%PAM-1.0

auth       required     pam_securetty.so
auth       requisite    pam_nologin.so
auth       include      system-local-login
+ auth       optional     pam_gnome_keyring.so
account    include      system-local-login
session    include      system-local-login
+ session    optional     pam_gnome_keyring.so auto_start
```

2. 再起動
3. デフォルトキーリングを確認

```bash
busctl --user call org.freedesktop.secrets /org/freedesktop/secrets/aliases/default org.freedesktop.DBus.Properties Get ss org.freedesktop.Secret.Collection Label
```

> [!TIP]
> **v s "Login"** と表示されれば良い。
