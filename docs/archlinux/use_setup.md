# システム構築

## 目次

- [ディスプレイマネージャー (greetd)](#ディスプレイマネージャー-greetd)

## ディスプレイマネージャー (greetd)

1. 編集: **/etc/greetd/config.toml**

```diff
- command = "agreety --cmd /bin/sh"
+ command = "tuigreet --remember --cmd 'uwsm start hyprland.desktop'"
```

2. 有効化

```console
$ sudo systemctl enable greetd.service
Created symlink '/etc/systemd/system/display-manager.service' + '/usr/lib/systemd/system/greetd.service'.
```
