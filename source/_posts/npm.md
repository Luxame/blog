---
title: NPM
date: 2016-07-22 23:06:22
tags: [Node.js, npm, nvm]
---

# 什麼是 NPM ?

NPM 全名叫 Node Package Manager ，是 Node.js 下的主流管理套件。

要使用 npm 很簡單，只要裝好 Node 就能把 npm 裝好了！

# 安裝 Node.js

## Mac OS X

使用 `brew install` 是最簡單的方式！

Homebrew 的安裝

[官方網站](http://brew.sh/)

因為 Node.js 社群活耀度還蠻高的，所以版本的更新速度還蠻快速，為了快速調整版本可以安裝 `NVM` 做管理。

### NVM 安裝

直接使用 Homebrew 安裝

```
$ brew install nvm
```

將下列指令加入 .bash_profile（或 .bashrc）檔案+

```
export NVM_DIR=~/.nvm

source $(brew --prefix nvm)/nvm.sh
```

重新載入 .bash_profile 設定

```
$ source .bash_profile
```

以上設定完成後，可以在終端機 `nvm` 查看是否有正確安裝
