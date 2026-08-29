# テーマ

## 目次

- [パッケージインストール](#パッケージインストール)
- [Qt 設定](#qt-設定)
  - [kvantum を設定](#kvantum-を設定)
  - [Qt に kvantum を設定](#qt-に-kvantum-を設定)
- [GTK 設定](#gtk-設定)

## パッケージインストール

- 公式リポジトリ

```bash
sudo pacman -S --needed kvantum kvantum-qt5 qt5ct qt6ct
```

- AUR

```bash
yay -S --needed catppuccin-gtk-theme-mocha kvantum-theme-catppuccin-git
```

## Qt 設定

### kvantum を設定

1. 起動: **kvantum**
2. タブ: **テーマの変更と削除**
3. ドロップダウン: **catppuccin-mocha-peach**
4. ボタン: **このテーマを使用**

### Qt に kvantum を設定

1. 編集: `~/.config/uwsm/env`

```diff
+ export QT_QPA_PLATFORMTHEME=qt6ct
```

2. 起動: **qt6ct**
3. タブ: **外観**
4. スタイル: **kvantum**
5. 配色: **Style's colors**
6. 標準ダイアログ: **XDG Desktop Portal**
7. ボタン: **適用** -> **OK**
8. **qt5ct** でも同じ設定を行う

## GTK 設定

1. ダークテーマを設定

```bash
gsettings set org.gnome.desktop.interface color-scheme 'prefer-dark'
```

2. テーマカラーを設定

```bash
gsettings set org.gnome.desktop.interface gtk-theme 'catppuccin-mocha-peach-standard+default'
```

3. ファイル作成

```bash
touch ~/.config/gtk-3.0/settings.ini
touch ~/.config/gtk-4.0/settings.ini
```

4. 編集: `~/.config/gtk-3.0/settings.ini` , `~/.config/gtk-4.0/settings.ini`

```ini
[Settings]
gtk-theme-name = catppuccin-mocha-peach-standard+default
gtk-application-prefer-dark-theme = 1
```

5. 編集: `~/.config/gtk-4.0/gtk.css`

```diff
+ @import url("/usr/share/themes/catppuccin-mocha-peach-standard+default/gtk-4.0/gtk.css");
```

6. 再起動
