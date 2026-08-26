# Keyring

## 目次

- [パッケージインストール](#パッケージインストール)
- [設定](#設定)

## パッケージインストール

```bash
sudo pacman -S --needed gnome-keyring libsecret
```

## 設定

1. 編集: ~/.config/uwsm/env

```diff
+ export SECRET_SERVICE_BUS_NAME=org.freedesktop.secrets
```

2. 編集: ~/.config/hypr/hyprland.lua

```diff
hl.on("hyprland.start", function () 
+  hl.exec_cmd("uwsm app -- gnome-keyring-daemon --start --components=secrets")
end)
```

3. 再起動
4. Choose password for new keyring: **(任意のパスワード)**  
  パスワードを入力すると新しいキーリングが作成される。

> [!TIP]
> PC のログインパスワードと同じパスワードを設定すると、ログイン時に自動的に解錠される。  
> ログインの度にキーリングを解錠する手間が省ける。
