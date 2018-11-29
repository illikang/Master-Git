* [Git分支全攻略](#Git分支全攻略)
    * [引入分支概念](#引入分支概念)
    * [开发中的分支策略](#开发中的分支策略)
    * [实际操作实例](#实际操作实例)
# Git分支全攻略

概要：结合实际工作场景，详细介绍了Git分支使用的基本策略与操作方法

## 引入分支概念
几乎所有的版本控制系统都以某种形式支持分支。 使用分支意味着你可以把你的工作从开发主线上分离开来，以免影响开发主线。 在很多版本控制系统中，这是一个略微低效的过程——常常需要完全创建一个源代码目录的副本。对于大项目来说，这样的过程会耗费很多时间。

有人把 Git 的分支模型称为它的“必杀技特性”，也正因为这一特性，使得 Git 从众多版本控制系统中脱颖而出。 为何 Git 的分支模型如此出众呢？ Git 处理分支的方式可谓是难以置信的轻量，创建新分支这一操作几乎能在瞬间完成，并且在不同分支之间的切换操作也是一样便捷。 与许多其它版本控制系统不同，Git 鼓励在工作流程中频繁地使用分支与合并，哪怕一天之内进行许多次。 理解和精通这一特性，你便会意识到 Git 是如此的强大而又独特，并且从此真正改变你的开发方式。

## 开发中的分支策略
在实际开发中，我们应该按照几个基本原则进行分支管理：
1. `master`分支应该是非常稳定的，也就是仅用来发布新版本，平时不能在上面干活；
2. `dev`分支是平时干活使用的分支，也就是说，`dev`分支是不稳定的。到某个时候，比如2.0版本发布时，在把dev分支合并到`master`上，在`master`分支发布2.0版本；
3. 开发团队买个人都在`dev`分支上干活，每个人都有自己的分支，时不时地往`dev`分支上合并就可以了。

所以，在团队合作的情况下，整个项目的分支看起来就像这样：
![](/img/group.png)

具体来说：
1. 远程分支：master和develop。master分支用于发布版本，非常稳定，一般不接受提交，只能从develop分支merge。develop分支用于开发，用于接受提交。开发完一个版本，由项目负责人将develop分支merge到master分支，然后发布新版本。
2. 本地分支：master、develop、以及自己的分支（例如Tom,Jim,feature1,bug1）。其中master和develop分支需要和远程同步。自己的分支部需要同步，在自己的分支开发完后，merge到develop，然后将本地develop提交到远程develop。git push origin remote_branch时，如果remote_branch不存在，会自动创建。

## 实际操作实例
1. 在Github（远程仓库）上创建初始化完成的项目GitTest，Github默认创建`master`分支。
2. 在`master`分支基础上，创建分支`dev`分支用于发开。（该步骤也可省略，因为本地仓库提交到远程仓库时，如果分支在本地存在而在远程不存在，则远程仓库会自动创建同名分支）
3. 在本地创建工作路径，并把远程库克隆到工作路径。有HTTPS和SSH两种克隆方式。
   * SSH需要提前完成设置，参考：
   * HTTPS方式

```
$ git clone http://git.oschina.net/illikang/GitTest.git
```
注意，采用clone的方法复制项目到本地以后，本地仓库自动与远程仓库建立连接。以后要上传更改内容，只需要git push origin branch就可以。

4. 项目克隆到本地以后，先查看项目分支情况:
```
$ git branch -a
//-r:显示远程分支
//-a:显示本地和远程分支
//*表示目前所在分支
 *master
>remotes/origin/HEAD -> origin/HEAD
>remotes/origin/dev
>remotes/origin/master
//可以看到，克隆以后的项目，有一个本地分支master和两个远程分支/origin/master和/origin/dev
```
5. 创建个人工作分支并切换到该分支：
```
$ git branch illikang
$ git checkout illikang
$ git branch -a
 *illikang
  master
>remotes/origin/HEAD -> origin/HEAD
>remotes/origin/dev
>remotes/origin/master
```
6. 开始自己的项目工作。
7. 完成部分更改后，在illikang分支下，查看状态,添加更新，并提交更新到本地仓库,注意每次提交更新前，一定要先查看当前分支。
```
$ git status
$ git add .
$ git commit -m "update on 25th"
```
8. 在本地仓库，将`illikang`分支的更新工作合并到本地`dev`分支。
```
//切换到dev分支
$ git branch dev
//将illikang分支合并到当前分支（dev）
$ git merge dev
```
9. 将更新后的本地仓库分支(dev)上传到远程仓库对应分支(dev)
```
git push origin dev
```
登陆Github查看远程仓库，发现远程仓库的dev分支已经与本地仓库的dev分支同步了。
10. 如果dev项目开发完成，在远程仓库的`dev`分支上提交申请合并到`master`分支。通过New pull request按钮创建pull request.
11. 进入远程项目的Pull requests管理提交的pull request。合并通过后删除分支，工作完成。
