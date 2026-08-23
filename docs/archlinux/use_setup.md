# システム構築

## 目次

- [ディスプレイマネージャー (greetd)](#ディスプレイマネージャー-greetd)
- [シェル (zsh)](#シェル-zsh)

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

## シェル (zsh)

1. 場所を確認

```console
$ command -v zsh
/usr/bin/zsh
```
2. 登録を確認

```console
$ cat /etc/shells
/bin/sh
/bin/bash
bin/rbash
/usr/bin/sh
usr/bin/bash
/usr/bin/r bash
/usr/bin/systemd-home-fallback-shell
/usr/bin/git-shell
/bin/zsh
/usr/bin/zsh
```

3. シェルを変更

```console
$ chsh -s $(command -v zsh)
Changing shell for user.
Shell changed.
```

4. 再起動

```bash
reboot
```
