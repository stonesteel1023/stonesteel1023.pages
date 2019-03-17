---

layout : post
date : 2019-03-17 21:00
title: "Installing zsh and setting for visual studio code"
comments: true
tag : shell

---

# oh-my-zsh install
> https://github.com/robbyrussell/oh-my-zsh

### installができたら.zshrcファイルが生成されたことを確認

# theme 設定
> https://github.com/robbyrussell/oh-my-zsh/wiki/External-themes

### zshが遅くなっちゃたら
>　https://github.com/robbyrussell/oh-my-zsh/issues/5327


# zshで”code”入力するとVisualStudioが起動する

.zshrcを編集
’nano ~/.zshrc’

以下を追記する

’’’
function code {
    if [[ $# = 0 ]]
    then
        open -a "Visual Studio Code"
    else
        local argPath="$1"
        [[ $1 = /* ]] && argPath="$1" || argPath="$PWD/${1#./}"
        open -a "Visual Studio Code" "$argPath"
    fi
}
’’’
追記したら、nanoを終了(＾X)して以下を実行

’$ source ~/.zshrc’

以上でcodeコマンドでVisual Studio Codeが起動される

例）
’code ~/.zshcr’

# yarn install & 設定

’brew install yarn’

.zshrcを編集
’nano ~/.zshrc’

以下を追記する
export PATH="$PATH:`yarn global bin`"
