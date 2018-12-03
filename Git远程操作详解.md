* [Git远程操作详解](#Git远程操作详解)
  * [概述](#概述)
  * [5个Git远程操作命令](#5个Git远程操作命令)
    * [git remote](#git remote)
    * [git clone](#git clone)
    * [git fetch](#git fetch)
    * [git pull](#git pull)
    * [git push](#git push)


# Git远程操作详解
## 概述
要参与任何一个Git项目的协作，必须要了解如何管理远程仓库。远程仓库是指托管在网络上的项目仓库，可能会有好多个，其中有些你只能读，另外有些可以写。同他人协作开发某个项目时，需要管理这些远程仓库，以便推送或拉取数据，分享各自的工作进展。管理远程仓库的工作包括：添加远程库、移除废弃的远程库，管理各式远程库分支，定义是否跟踪这些分支等等。
Git有很多优势，其中之一就是远程操作非常简便。Git的远程操作用下图可以基本概括：
![remote](img/remote.jpg)
本文详细介绍5个Git命令的概念和用法，理解了这些内容，你就会完全掌握Git远程操作：
 1. [git remote](#git remote)
 2. [git clone](#git clone)
 3. [git fetch](#git fetch)
 4. [git pull](#git pull)
 5. [git push](#git push)

## 5个Git远程操作命令
### git remote
  * git remote 命令用于管理本地仓库跟踪的一组远程仓库，包括为查看本地仓库配置的远程库，为本地仓库添加跟踪远程库，移除本地仓库跟踪的远程库等等。
  * 语法总结
  ```
  git remote [-v | --verbose]  //查看当前配置的远程库，加V表示列出详细信息，在每一个名字后面列出其URL
  git remote add [-t <branch>] [-m <master>] [-f] [--[no-]tags] [--mirror=<fetch|push>] <name> <url>
  //添加一个新的远程库
  git remote rename <old> <new>  //重命名一个远程仓库
  git remote remove <name>    //移除一个远程仓库
  git remote set-head <name> (-a | --auto | -d | --delete | <branch>)
  git remote set-branches [--add] <name> <branch>
  git remote get-url [--push] [--all] <name>
  git remote set-url [--push] <name> <newurl> [<oldurl>]
  git remote set-url --add [--push] <name> <newurl>
  git remote set-url --delete [--push] <name> <url>
  git remote [-v | --verbose] show [-n] <name>…​
  git remote prune [-n | --dry-run] <name>…​
  git remote [-v | --verbose] update [-p | --prune] [(<group> | <remote>)…​]
  ```
  * 示例
  ```
  //查看当前库配置的远程库
  $ git remote -v
  $ git remote add origin git@github.com:illikang/BigData-Learning-Notes.git
  $ git remote rename origin bigdata
  $ git remote remove bigdata
  ```
### git clone
  * git clone 命令将远程仓库克隆到本地目录中，为克隆的存储库中的每个分支创建远程跟踪分支（使用git branch -r可以查看，也就是说clone命令实际上是包含了remote add命令的），并从克隆检出的存储库作为当前活动分支的初始分支。
  * 语法
  ```
  git clone [--template=<template_directory>]
      [-l] [-s] [--no-hardlinks] [-q] [-n] [--bare] [--mirror]
      [-o <name>] [-b <name>] [-u <upload-pack>] [--reference <repository>]
      [--dissociate] [--separate-git-dir <git dir>]
      [--depth <depth>] [--[no-]single-branch]
      [--recurse-submodules] [--[no-]shallow-submodules]
      [--jobs <n>] [--] <repository> [<directory>]
  //常用命令
  git clone <远程库的地址>
  git clone <远程库的地址> <本地目录名>
  //重要：在克隆之后，没有参数的普通git pull命令将更新所有远程跟踪分支，并且没有参数的git pull将另外将远程主分支合并到当前主分支（也就是默认包含了merge命令）
  ```
  * 示例
  ```
  $ git clone http[s]://example.com/path/to/repo.git
  $ git clone http://git.oschina.net/yiibai/sample.git
  $ git clone ssh://example.com/path/to/repo.git
  $ git clone git://example.com/path/to/repo.git
  $ git clone /opt/git/project.git
  $ git clone file:///opt/git/project.git
  $ git clone ftp[s]://example.com/path/to/repo.git
  $ git clone rsync://example.com/path/to/repo.git
  ```
  * 补充：在某些场合，Git会自动在本地分支与远程分支之间，建立一种追踪关系(tracking)。比如，在git clone的时候，所有本地分支默认与远程主机的同名分支建立追踪关系，例如本地的master分支自动“追踪” origin/master分支。在有跟踪关系的情况下，git pull命令就可以省略远程分支名。如果当前分支只有一个追踪分支，连远程主机名都可以省略
  ```
  $ git pull
  ```
  Git也允许手动建立追踪关系：
  ```
  $ git branch --set-upstream master origin/next
  ```
  上面命令指定`master`分支追踪`origin/next`分支。
### git fetch
  * git fetch命令用于从远程库下载对象和引用。远程跟踪分支已更新，需要将这些更新取回本地，这时就要用到git fetch 命令。
  * 语法
  ```
  git fetch [<options>] [<repository> [<refspec>…]]
  git fetch [<options>] <group>
  git fetch --multiple [<options>] [(<repository> | <group>)…]
  git fetch --all [<options>]
  ```
  * 示例
  ```
  $ git fetch origin dev   //从远程库的dev分支取更新文件
  $ git fetch   //将远程库的全部更新取回本地
  ```
### git pull
  * git pull 命令用于取回远程主机某个分支的更新，再与本地的制定分支合并。在默认模式下，git pull是git fetch 后跟 git merge FETCH_HEAD的缩写。更准确地说，git pull使用给定的参数运行git fetch，并调用git merge将检索到的分支头合并到当前分支中。使用--rebase，它运行git rebase而不是git merge。
  * 语法
  ```
  git pull [options] [<repository> [<refspec>…]]
  ```
  * 示例
  ```
  $ git pull <远程主机名> <远程分支名>:<本地分支名>
  $ git pull origin next:master
  $ git pull origin next

  ```
### git push
  * gi
  * gi
  * gi
