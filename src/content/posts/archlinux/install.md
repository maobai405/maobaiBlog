---
title: archlinux安装
published: 2025-04-10
description: 安装archlinux
tags: [archlinux]
category: archlinux
draft: false
lang: zh_CN
---

# 安装系统

使用 archinstall 安装 ，选择 hyprland 桌面
安装完成后直接 exit 退出安装，reboot 重启

# 安装必要软件

## 配置 archlinuxcn 源

```fish
sudo vim /etc/pacman.conf
```

在最后添加

```fish
[archlinuxcn]
Server = https://mirrors.tuna.tsinghua.edu.cn/archlinuxcn/$arch
```

保存退出后安装 archlinuxcn-keyring

```fish
sudo pacman -S archlinuxcn-keyring
sudo pacman -Sy
```

## 安装软件

```fish
sudo pacman -S paru yay

paru -S ttf-maplemono-nf-cn zen-browser dingtalk-bin fish mise nvim fzf ripgrep fd yazi neovide starship lsd pnpm

```

### 更改 shell 为 fish

```fish
sudo vim ~/.bashrc

# 在文件内添加fish
```

### 安装 node 包 rust 包

```fish
mise install node
mise install rust
mise use -g node
```

## 代理

```fish
paru -S v2ray v2raya

systemctl enable v2raya --now
```

## 输入法

```fish
paru -S fcitx5 fcitx5-rime rime-ice-git
```

### 配置

1.打开 Rime 配置文件夹/home/ccrice/.local/share/fcitx5/rime 2.新建文件 default.custom.yaml 3.在文件中写入以下内容

```bash

patch:
  # 引用「雾凇拼音」配置: /usr/share/rime-data/rime_ice_suggestion.yaml
  __include: rime_ice_suggestion:/
  # 自定义配置
  __patch:
    # 候选词个数
    menu/page_size: 7
    # 绑定键样式:「输入法模式-接收键-触发事件」
    key_binder/bindings/+:
      # 开启逗号句号翻页
      - { when: paging, accept: comma, send: Page_Up }
      - { when: has_menu, accept: period, send: Page_Down }
```
