---
layout: post
title: rbenv、fish 與 VSCode 設置之路
comments: true
---

在最新的 VSCode 1.3.1 版裡，Integrated Terminal 變得更加好用，但由於上游套件 `xterm.js` 的緣故，`zsh` 還是有無法捲動的問題。不過作為一個 Rails 開發者，VSCode 已經是個足夠好用的、足矣媲美 Sublime Text 的編輯器了。為了能夠在 VSCode 開發 Rails，特別對目前的開發環境做出調整。

## 由 RVM 轉換至 rbenv

聽聞 [rbenv][rbenv] 相較 rvm 來說，是對系統較為乾淨的 ruby 版本管理工具。就趁這次更新開發環境一起重置吧！

### 移除 rvm

套件也內建了一鍵移除的方式：

```bash
rvm implode
```

做完之後也可以將 rvm 剩餘的檔案移掉：

```bash
rm -rf ~/.rvm
```

### 安裝 rbenv

參照了[專案官網][rbenv]的說明，只要把該 repo clone 下來，設置好 `PATH` 變數，在為 shell 加上一些設定便可以開始安裝 ruby 了。在 OS X 環境又更容易，只要用 Homebrew 一鍵安裝便成。

```bash
brew install rbenv
```

裝完之後就可以發現 rbenv 已經被裝到 Homebrew 的可執行位置了，可以跑 `which` 指令檢驗

```bash
which rbenv
# => /usr/local/bin/rbenv
```

在 Homebrew 安裝跑完之後，安裝腳本會提示已在 shell profile(.zshrc/.profile/.bash_profile) 加上 rbenv 初始化腳本，預設內容如下：

```bash
export PATH="$HOME/.rbenv/bin:$PATH"
eval "$(rbenv init -)"
```

第二行的 `rbenv init -` 指令輸出，在你一般開啟的 Terminal 會跑出：

```bash
export PATH="/Users/USERNAME/.rbenv/shims:${PATH}"
export RBENV_SHELL=zsh
source '/usr/local/Cellar/rbenv/1.0.0/libexec/../completions/rbenv.zsh'
# ... 下面一串
```

不過在 VSCode 內建終端機卻會跑出：

```bash
export PATH="/usr/local/opt/rbenv/shims:${PATH}" # 就這行不同
export RBENV_SHELL=zsh

# ... 下面一樣
```

大概是執行權限不同的關係吧，rbenv 執行的位置不同也會讓 `gem`/`bundler` 裝到不同的位置，為了方便起見，一律設成一般終端機跑出來的那串。把下面這串貼到 `~/.zshrc` 最下面就是。

```bash
export PATH="/Users/USERNAME/.rbenv/shims:${PATH}"
export RBENV_SHELL=zsh
source '/usr/local/Cellar/rbenv/1.0.0/libexec/../completions/rbenv.zsh'
command rbenv rehash 2>/dev/null
rbenv() {
  local command
  command="$1"
  if [ "$#" -gt 0 ]; then
    shift
  fi

  case "$command" in
  rehash|shell)
    eval "$(rbenv "sh-$command" "$@")";;
  *)
    command rbenv "$command" "$@";;
  esac
}
```

若是 fish 就貼下面這個：

```
setenv PATH '/Users/USERNAME/.rbenv/shims' $PATH
setenv RBENV_SHELL fish
. '/usr/local/Cellar/rbenv/1.0.0/libexec/../completions/rbenv.fish'
command rbenv rehash 2>/dev/null
function rbenv
  set command $argv[1]
  set -e argv[1]

  switch "$command"
  case rehash shell
    . (rbenv "sh-$command" $argv|psub)
  case '*'
    command rbenv "$command" $argv
  end
end
```

### 裝 ruby

```bash
rbenv install 2.3.1
rbenv global 2.3.1
gem install bundler

bundle install
```

搞定！

### Pow 的額外設定

其實在 Pow 的 [wiki](https://github.com/basecamp/pow/wiki/Troubleshooting) 就有解答，不過因為我又對 rbenv 做了 PATH 的改動，所以設定也不太一樣。

打開 ~/.powconfig 並貼入

```bash
export PATH="/Users/USERNAME/.rbenv/shims:/Users/USERNAME/.rbenv/shims/bin:$PATH"
```

然後重啟 pow service

```bash
touch ~/.pow/restart.txt
```

## 將主要 shell 由 zsh 換成 fish

zsh 有著非常有名的 [oh-my-zsh][oh-my-zsh] 設定檔管理框架，fish 底下也有個好用的叫 [fisherman][fisherman]。不過我終究是沒有把系統預設的 shell 換掉(`chsh` 指令)，因為挺多工具與 fish 不相容。

fish 啟動速度飛快，帶我重回還沒有裝一堆 zsh 套件的美好往日時光。

### VSCode

講來講去這篇好像跟 VSCode 沒啥關係耶？那就附張截圖好了，VSCode 也是十分優秀的 Markdown 編輯器，內建了快速的 Preview，碼 code、寫文件、Terminal、Task Runner 全都內建了，還夠輕量快速，Extension 也陸續多了起來，還真不好挑剔啊 XD。

![][screenshot1]


[rbenv]: https://github.com/rbenv/rbenv
[oh-my-zsh]: https://github.com/robbyrussell/oh-my-zsh
[fisherman]: https://github.com/fisherman/fisherman
[screenshot1]: https://lh3.googleusercontent.com/-o17HvZ1w7k6NzK0kw84uJNdhTmnWCH1hGyHduSokRnhgdeoaIh8reqDVrlAc43uGIN9K5KZYpujZuuSdqrV5BiuBhYNsQlubHEfAPut1pGvbkYzw3zKLfuEpphlj7JpBKj9x3m-Zk-rA4s-xFXnagC3vO8otXWOAH7qXYg47kx8D1qHVDpwHjD4WW8yd1jV_CYZlL5VWfrbbiVAdlwBDalB_UexT8emkhqTMjNwQMhIwMyqEXk8LcsfuI5IqgpdR488KovW9Z3ckMJjMbMWSpYOUfMuJ4_pRgYPMla9cm-h6QcP5PXIyhfTERUTRUyUAawcJAWC74R519UVe_gLizh0RqvKazWU0ISCjXVOmbH_bljBAwRIBZobMme9W3deBztAMd0asjKFJnjsviDKDgb1n5y8wS6z5nfNGWijNJbl2JwpVv9EVStbGM4OoqM-9st2MO5oVZJEK8PgtNp1LvuBr3Qkch_k2RXnSZJTfxs4aYZroTJTplZI2XEZS8bkK6Xlczod9rFAvsBdSK2G1A3rixmyBrTOkb7YE7Z8E5NRGWXx3mJns607eu00e69bzGlSe7kc_IpwN1wiYUtqWuRjABPqIhsV=w1046-h781-no
