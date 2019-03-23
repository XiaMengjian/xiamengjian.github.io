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

### dependencies和devDependencies的区别
除了字面的含义的区别，

<<<<<<< HEAD
dependencies和devDependencies的区别还在于：

如果你的项目是发布到npm的一个包，
那么这个包的package.json中的dependencies中的依赖是会被下载下来到这个包的node_modules文件夹中的（如果你的项目本身没有这个依赖），而devDependencies不会。

举个例子：
我发布了一个组件A，它有dependencies：lodash和devDependencies：moment。
那么，如果你的项目npm install 了组件A。
除非你的项目也依赖了lodash并且版本一致，那么项目的node_modules/A下会有一个node_modules，里面会有lodash。
而 moment，则无论如何也不会出现在你的项目中。

至于一般的项目，不管你是安装在dev还是dependencies中，安装的时候都会安装，打包的时候都会被打进去的，区分依赖只是为了让项目看起来更加清晰。
=======
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


 
>>>>>>> 09325c625530c1c849754b375ba6d686acce8f23

