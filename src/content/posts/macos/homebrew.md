---
title: MacOS下安装 HomeBrew以及软件的安装和备份
published: 2024-11-30
description: 关于MacOS下包管理工具的安装和备份
tags: [MacOS, HomeBrew]
category: 系统
draft: false
---

## 安装 HomeBrew

终端中执行下面的安装脚本-[**_脚本引用至知乎文章_**](https://zhuanlan.zhihu.com/p/111014448/)

安装：

```bash
/bin/zsh -c "$(curl -fsSL https://gitee.com/cunkai/HomebrewCN/raw/master/Homebrew.sh)"
```

卸载：

```bash
/bin/zsh -c "$(curl -fsSL https://gitee.com/cunkai/HomebrewCN/raw/master/HomebrewUninstall.sh)"
```

## 软件备份及恢复

基于[**brew bundle**](https://github.com/Homebrew/homebrew-bundle)实现, 它可以生成一个软件的安装列表

可以备份的软件以下类型:

1.  brew tap 中的软件库
2.  brew 安装的命令行工具
3.  brew cask 安装的 App
4.  Mac App Store 安装的 App
5.  VScode 插件

上述列表中前三类软件可以直接备份和恢复, App Stroe 安装的软件需要额外安装 [**mas**](https://github.com/mas-cli/mas) 进行支持, VScode 插件不建议使用 HomeBrew 管理不过多赘述

如果需要使用 HomeBrew 备份和恢复 App Stroe 软件需要在备份前先对 mas 进行安装, HomeBrew 不自带这个软件

安装 mas:

```bash
brew install mas
```

详细的命令可以在终端中运行 mas help 查看

## 备份

```bash
brew bundle dump --describe --force --file="~/.config/Brewfile"
```

`--describe`：为列表中的命令行工具加上说明性文字。

`--force`：直接覆盖之前生成的 Brewfile 文件。如果没有该参数，会询问是否覆盖。

`--file="~/.config/Brewfile"`：在指定位置生成文件。如果没有该参数，则在当前目录生成 Brewfile 文件。

生成的文件如下(自用):

```bash
tap "homebrew/core"
tap "homebrew/bundle"
tap "homebrew/cask"
tap "homebrew/services"
tap "homebrew/cask-fonts"
tap "koekeishiya/formulae"
tap "felixkratz/formulae"
tap "leoafarias/fvm"

# 终端下载AppStore应用
brew "mas"
mas "Xcode", id: 497799835

# fish
brew "fish"
# sehll提示以及美化
brew "starship"
# 雄心勃勃的 Vim-fork 专注于可扩展性和敏捷性
brew "neovim"
# 平铺窗口管理器
brew "koekeishiya/formulae/yabai"
# 快捷键管理工具
brew "koekeishiya/formulae/skhd"
# 额外的状态栏
brew "felixkratz/formulae/sketchybar"
# 编写的超快终端文件管理器
brew "yazi"
# node包管理工具
brew "pnpm"
# 简单、快速和用户友好的替代方案
brew "fd"
# 网络代理软件
cask "sfm"
# node 包管理器
brew "mise"
# 用 Go 编写的命令行模糊查找器
brew "fzf"
# 轻量级灵活的命令行 JSON 处理器
brew "jq"
# 用于 git 命令的简单终端 UI
brew "lazygit"
# 终端 docker gui工具
brew "lazydocker"
# 搜索工具，如 grep 和 The Silver Searcher
brew "ripgrep"
# 文件替换工具
brew "gnu-sed"
# ls 增强
brew "lsd"
# 可插拔终端工作区，以终端复用器为基本功能
brew "zellij"
# 网络流量监控工具
brew "ifstat"
# 管理flutter版本工具
brew "leoafarias/fvm/fvm"

# 字体

# 浏览器
cask "arc"
# visual-studio-code
cask "visual-studio-code"
# 开关统一管理
cask "only-switch"
# 用于文本翻译和识别的软件
cask "easydict"
# 终端模拟器
cask "kitty"
# 文件管理器
cask "qspace-pro"
# 远程桌面
cask "rustdesk"
# 音视频播放器
cask "iina"
# 柠檬清理
cask "tencent-lemon"
# Plug-in productivity tool set
cask "utools"
# 微信
cask "wechat"
# 企业微信
cask "wechatwork"
# 截图
cask "pixpin"
```

## 恢复

```bash
brew bundle --file="~/.config/Brewfile"
```
