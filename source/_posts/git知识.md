---
title: git知识
date: 2019-03-21 16:50:26
tags:
---


# 最小配置
配置user.name,user.email

```
git config --global user.name 'myName'
git config --global user.email 'myEmail'
``` 
### global 的作用：
变更信息上带上用户信息，可以方便快速的查找到作者，同时能够发送邮件到相应email

除了 global之外 `git config` 还能设置
1. local  只针对某个仓库       默认
2. global 当前用户所有仓库有效
3. system 对系统所有登录用户有效

查看config配置信息，添加 --list 如果没指定范围（--global） 默认显示所有的配置
`git config --list --global`
`git config --list --local` _需要在对应的git工程里_
![](/media/15531595805175.jpg)


# 建立仓库
`git init` or `git init your_project` (无代码情况)

```
git add <files>  文件加入到git跟踪 此时文件是在stage中
git add -u 对于已经跟踪的文件update 可以通过-u 来加入暂存区
git commit -m '' 

git checkout -- <file> 丢弃暂存区的某个文件
git rm --cached <file> 剔除暂存区 to unstage
git reset HEAD <file>  to unstage

git status 查看工作区以及暂存状态
git log (--pretty=oneline)看日志
```


## 暂存区概念

![暂存区](/media/15531607337438.jpg)


 

