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

查看config配置信息，添加 --list
`git config --list --global`
![](/media/15531595805175.jpg)




