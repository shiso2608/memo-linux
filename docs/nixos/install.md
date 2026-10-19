# インストール手順

## 目次

- [インストール](#インストール)
  - [ブートローダー](#ブートローダー)
  - [NisOS Installer](#nisos-installer)
    - [Welcome](#welcome)
    - [Location](#location)
    - [Keyboard](#keyboard)
    - [Users](#users)
    - [Desktop](#desktop)
    - [Unfree software](#unfree-software)
    - [Partitions](#partitions)
    - [Summary](#summary)
    - [Install](#install)
    - [Finish](#finish)

## インストール

### ブートローダー

- **NixOS (バージョン番号) Installer GNOME (Linux (バージョン番号))**

### NisOS Installer

#### Welcome

- **American English**

> [!NOTE]
> インストール完了後に日本語に変更する。

#### Location

- Region: **Asia**
- Zone: **Tokyo**

#### Keyboard

- Keyboard model: **Generic 104-key PC**
- **English (US)**
- **Default**

> [!NOTE]
> US 配列キーボードを使用。

#### Users

- What is your name?: **(自由)**
- What name do you want to use to log in?: **(自由)**
- Choose a password to keep your account safe.: **(自由)**
- Require strong passwords.: **(チェック)**
- Use the same password for the administrator account.: **(チェック無し)**
- Choose a password for the administrator account.: **(自由)**

#### Desktop

- **GNOME**

#### Unfree software

- Allow unfree software: **(チェック無し)**

> [!NOTE]
> 個別に許可する運用を実施する。

#### Partitions

- Select storage device: **(ストレージ)**
- Erase disk: **(チェック)**
- **Swap (no Hibernate)**, **btrfs**
- Encrypt system: **(チェック無し)**
- Bootloader location: **Master Boot Record of (ストレージ)**

#### Summary

> [!TIP]
> 設定した内容を確認する。

#### Install

> [!TIP]
> **Toggle log** をクリックすると作業内容を確認できる。

#### Finish

- Restart now: **(チェック無し)**

> [!IMPORTANT]
> シャットダウンをして、インストールメディアを取り外す。
