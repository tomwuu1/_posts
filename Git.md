---
title: Git
date: 2024-07-29 10:22:04
tags:
---

# log,reflog

`git log` 命令用于显示提交历史记录。它会列出仓库的提交历史，包括每次提交的哈希值、作者、日期和提交信息。

`git reflog` 命令用于查看 Git 仓库的引用日志。Reflog 记录了所有对仓库的引用更新的操作，包括那些在正常 `git log` 中看不到的分支变更、重置和变基操作。这对恢复误操作（如误删分支或错误的 reset）非常有用。

# add,commit,push

```
工作区 (Working Directory)
  |
  | git add
  |
暂存区 (Staging Area)
  |
  | git commit
  |
本地版本库 (Local Repository)
  |
  | git push
  |
远程版本库 (Remote Repository)

```

# diff

1. 工作区和暂存区`git diff`
2. 暂存区和本地仓库的区别`git diff --cached`
3. 比较不同分支，标签之间的差异



# 分支

1. 创建分支`git branch <branch-name>`
2. 创建并切换分支`git checkout -b <branch-name>`
3. 切换分支`git checkout <branch-name>`
4. 合并分支`git merge <branch-name>`
5. 删除分支`git branch -d <branch-name>`

# merge

## 遇到冲突

手动编辑文件，用`git add`标记将冲突已解决

还有像kdiff3等合并工具

# fetch

`git fetch`

获取远程分支，但是不和本地分支合并

# config

# reset

1. soft模式：将HEAD移动到指定的提交，但保留暂存区和工作区的更改
2. mixed模式（默认）：将HEAD移动到指定的提交，重置暂存区使其与指定的提交一致，保留工作目录的更改。
3. hard模式：将HEAD移动到指定的提交，重置暂存区和工作区

# revert

通过创建新的提交（引入与指定提交相反的更改），来撤销指定提交的影响

# rebase

看不懂

# cherry-pick

选择若干个特定的提交，将它们应用到当前分支上

# 创建一个空白新分支

