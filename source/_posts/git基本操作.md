---
title: git基本操作
date: 2020-06-20 13:35:39
categories: Git
keywords: git基本操作
description: git基本操作
---

## 克隆

克隆远程仓库，把服务器的代码拉到本地工作目录。

### 全克隆

```bash
git clone 远程仓库地址
```

找个会默认把一次性的把所有分支拉取下来到本地。好处就是方便切换，但是消耗时间很长，而且会占用很大的磁盘空间。

### 单一克隆

```bash
git clone -b branch1 远程仓库地址
```

这条命令看上去像是只拉取指定的分支：branch1。其实不然，它和全克隆一样，都会拉取所有代码。只是拉取后所在的分支就是指定的分支。所以需要配合其他一些命令参数来达到这种效果。

```bash
git clone -b branch2 --single-branch 远程仓库地址
```

找个命令行参数代表着单一克隆。只会拉取-b参数指定的分支。

### 深度克隆

因为git保存了很多的历史提交信息，我们可以通过`--depth num`来指定拉取最近几次提交的代码。大大减少占用磁盘的空间和加大拉取速度。但是一般也是配合使用。

```3bash
git clone -b branch3 --single-branch --depth 1 远程仓库地址
```

## 查看文件状态

```bash
git status

git diff
```

## 将工作区的文件添加到缓存区

```bash
git add .
```

## 提交

```bash
git commit -m "将缓存区的文件提交到本地仓库"

git commit -a "两步合成一步，也就是add commit 一条命令解决"
```

## 查看历史提交记录

```bash
# 长命令
git log

# 短命令
git log --oneline
```

## 删除和重命名

### 手动

```bash
# 手动删除
rm -rf file

# add
git add .

# commit
git commit -m "delete"

# 手动重命名
mv file1 file2

# add
git add .

# commit
git commit -m "mv"
```

### git 自动

```bash
git rm file

git commit -m "delete"

git mv file1 file2

git commit -m "mv"
```

## 暂存

当我们手头上的工作完成部分时，但不想提交，却又想切换分支，就可以先把此时的工作保存到一个栈上。

### 创建

```bash
jw199@DESKTOP-2OG3D2D MINGW64 /c/my_code/git_study (b7)
$ echo "git stash" > b7.txt

# 保存到栈中
jw199@DESKTOP-2OG3D2D MINGW64 /c/my_code/git_study (b7)
$ git stash
Saved working directory and index state WIP on b7: 38193c7 b7.txt
```

加注释信息保存：

```bash
jw199@DESKTOP-2OG3D2D MINGW64 /c/my_code/git_study (b7)
$ git stash push -m "update 1.txt"
Saved working directory and index state On b7: update 1.txt

jw199@DESKTOP-2OG3D2D MINGW64 /c/my_code/git_study (b7)
$ git stash list
stash@{0}: On b7: update 1.txt
stash@{1}: WIP on b7: 38193c7 b7.txt
stash@{2}: WIP on b7: 38193c7 b7.txt
```

### 查看

```bash
jw199@DESKTOP-2OG3D2D MINGW64 /c/my_code/git_study (b7)
$ git stash list
stash@{0}: WIP on b7: 38193c7 b7.txt
```

### 应用

当我们切换到其他分之后，回到切换前的分支，我们要将工作内容恢复，就可以从栈中取出来。

#### pop

```bash
# 保存的栈中
jw199@DESKTOP-2OG3D2D MINGW64 /c/my_code/git_study (b7)
$ git stash
Saved working directory and index state WIP on b7: 38193c7 b7.txt

# 查看工作区状态
jw199@DESKTOP-2OG3D2D MINGW64 /c/my_code/git_study (b7)
$ git status
On branch b7
nothing to commit, working tree clean

# 查看栈中的stash
jw199@DESKTOP-2OG3D2D MINGW64 /c/my_code/git_study (b7)
$ git stash list
stash@{0}: WIP on b7: 38193c7 b7.txt

# 取出来
jw199@DESKTOP-2OG3D2D MINGW64 /c/my_code/git_study (b7)
$ git stash pop
On branch b7
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   b7.txt

no changes added to commit (use "git add" and/or "git commit -a")
Dropped refs/stash@{0} (852b6d730a000d8d7393027f91a766313c2e8cbd)

jw199@DESKTOP-2OG3D2D MINGW64 /c/my_code/git_study (b7)
$ git status
On branch b7
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   b7.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

#### apply

与pop区别就是，取出来之后并不会删除stash。所以要手动删除

### 删除

```bash
# 删除索引为0的stash
$ git stash drop  0
Dropped refs/stash@{0} (46d5fef30728f1ff802ac254af522191a11387f3)

jw199@DESKTOP-2OG3D2D MINGW64 /c/my_code/git_study (b7)
$ git stash list

```

