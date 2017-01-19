title: 强大的zsh
date: 2015-12-30 18:19:18
tags: [终端]
---

## 安装`oh-my-zsh`
```bash
wget https://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh -O - | sh
```
用命令检查Mac 检查系统支持哪些终端
```bash
cat /etc/shells

<!-- 显示结果： -->
/bin/bash
/bin/csh
/bin/ksh
/bin/sh
/bin/tcsh
/bin/zsh
```


## mac中`zsh`命令,用sublime打开指定文件
`zsh`配置清单
```bash
alias subl="'/Applications/Sublime Text.app/Contents/SharedSupport/bin/subl'"
alias nano="subl"
export EDITOR="subl"


alias -s markdown=subl //指定markdown格式
alias -s md=subl //在命令行直接输入后缀为md的文件名，会在sublime text中打开
alias -s log=vim
alias -s txt=vim
alias -s gz='tar -xzvf' //表示自动解压后缀为 gz 的压缩包
alias -s tgz='tar -xzvf'//表示自动解压后缀为 tgz 的压缩包
```

## 自带插件快捷键

`d`,按下`d`命令会显示最近的历史记录
`zsh_stats`,按下`zsh_stats`会显示你的使用频率前 20 的命令


