---
title: temp
date: 2019-03-24 20:59:54
tags:
---

# git 重命名文件 git mv
 readme -> readme.md
方式1 

```
mv readme readme.md
git add readme.md
git rm readme
```
 方式2 git mv
 `git mv readme readme.md `
 
# 查看版本演变史 git log

```
git log 当前分支的信息
git log --all 所有分支的commit信息演进历史
git log --all --graph  带图形化信息
git log --oneline 一行简洁
git long -n4    最近的4条
```
# git commit

```
git commit -am 'msg' 直接把文件提交 不需要用add
```
 
 
# 探秘 .git 目录
1. HEAD: 里面引用的是当前正在工作的分支是谁
2. config: git config local设置的信息
3. refs: tags(标签，里程碑) heads(分支)
4. objects: 


# commit tree blob 之间的关系
![](/media/15534346860586.jpg)
commit tree -> 那个时间点文件的快照
tree 文件夹
blob 文件


```
git reset --hard 暂存区工作区所有的修改都取消
git cat-file -t 'commitid' -t 查看类型  -p 查看内容
```


# 分离头指针 detached HEAD （变更没有基于某个branch去做）
  git checkout 'commitId'
这时候也是可以commit, 但是没有和某个分支绑定，会被清除
git branch <new_branch> 

# HEAD branch 