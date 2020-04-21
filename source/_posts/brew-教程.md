---
title: brew 教程
date: 2020-02-26 10:34:22
tags: 
- brew
---
## 替换源

- 替换brew.git  

``` bash
cd "$(brew --repo)"
git remote set-url origin https://mirrors.ustc.edu.cn/brew.git
# git remote set-url origin https://mirrors.aliyun.com/homebrew/brew.git
```

- 替换homebrew-core.git

``` bash
cd "$(brew --repo)/Library/Taps/homebrew/homebrew-core"
git remote set-url origin https://mirrors.ustc.edu.cn/homebrew-core.git
# git remote set-url origin https://mirrors.aliyun.com/homebrew/homebrew-core.git
```

- 刷新源

``` bash
brew update
```

## 替换Homebrew Bottles源

``` bash
export HOMEBREW_BOTTLE_DOMAIN=https://mirrors.ustc.edu.cn/homebrew-bottles
# export HOMEBREW_BOTTLE_DOMAIN=https://mirrors.aliyun.com/homebrew/homebrew-bottles
```

## brew 常用命令

``` bash
brew doctor
brew update
brew outdated
brew cleanup
brew upgrade [FORMULA]
brew remove [FORMULA]
brew deps --installed --tree
brew deps [FORMULA] | xargs brew remove --ignore-dependencies
brew missing | cut -d: -f2 | sort | uniq | xargs brew install
```
