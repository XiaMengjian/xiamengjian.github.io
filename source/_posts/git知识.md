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

### dependencies和devDependencies的区别
除了字面的含义的区别，

dependencies和devDependencies的区别还在于：

如果你的项目是发布到npm的一个包，
那么这个包的package.json中的dependencies中的依赖是会被下载下来到这个包的node_modules文件夹中的（如果你的项目本身没有这个依赖），而devDependencies不会。

举个例子：
我发布了一个组件A，它有dependencies：lodash和devDependencies：moment。
那么，如果你的项目npm install 了组件A。
除非你的项目也依赖了lodash并且版本一致，那么项目的node_modules/A下会有一个node_modules，里面会有lodash。
而 moment，则无论如何也不会出现在你的项目中。

至于一般的项目，不管你是安装在dev还是dependencies中，安装的时候都会安装，打包的时候都会被打进去的，区分依赖只是为了让项目看起来更加清晰。

