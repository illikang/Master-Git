* [Git版本管理](#Git版本管理)
    * [概述](#概述)
    * [版本回退操作](#版本回退操作)

# Git版本管理
## 概述
前面我们掌握了如何创建Git本地项目，如何将本地修改提交到Git仓库等操作。有了这些基础就可以随意修改项目中的文件了。假如现在突然发现某个文件改错了，想回退到修改之前的版本，该如何做呢？接下来就来详细介绍Git的版本管理。
## 版本回退操作
  1.  查看提交记录
  ```
  $ git log
  //比如我这里查看的结果如下：
  commit 0b26ea70b84b873af7bb6af9a65dc93fcfe12a1f (HEAD -> temp, origin/temp)
  Author: leon <illikang@163.com>
  Date:   Tue Dec 4 02:08:02 2018 +0800

      add linux

  commit 75d416ef07162382ae80bbc1be5e06005d13ffc7
  Author: leon <illikang@163.com>
  Date:   Mon Dec 3 16:25:20 2018 +0800

      start Hbase

  commit 366be82a75657861cdb2694827807bbcf9e5adf9
  Author: leon <illikang@163.com>
  Date:   Mon Dec 3 14:44:32 2018 +0800

      java redis

  commit c7c162a6b29e2110435b3a52c24f9ab47c1443a1
  Author: leon <illikang@163.com>
  Date:   Mon Dec 3 03:19:33 2018 +0800

      finish Redis 数据类型及其命令
  ```
  2. 解读日志，查看日志中提交的修改提示信息，修改提交时间，确定要回退的版本（由此可见每次提交时的提示信息多么重要），比如，现在要回退到“finish Redis 数据类型及其命令”的版本。
  3. 回退历史版本。如何告诉Git我要回退的版本呢？在Git中，用`HEAD`表示当前版本，也就是上面最新提交的“0b23ea..”，上个版本就是`HEAD^`,上上个版本就是`HEAD^^`，上100个版本就是`HEAD~100`，根据步骤2，要回退的版本是`HEAD^^`：
  ```
  $ git reset --hard HEAD^^
  ```
  4. 回退未来版本。步骤3后，我们回退到了`HEAD^^`版本。于是，`HEAD`和`HEAD^`就相当于是未来版本了。假如现在回退到`HEAD^^`以后突然发现，我其实要去的是`HEAD^`版本，那该如何回退到这个未来版本呢？

  其实，除了用“HEAD”来表示版本信息，Git还可以识别commit id的版本信息，就是commit后的长传文字：“366be82a75657861cdb2694827807bbcf9e5adf9”。当然实际操作的时候，版本号没必要写全，前几位就可以了，Git会自动去找。
  ```
  $ git reset --hard 366be
  ```
  5. 如果因为关机，无法看到刚刚执行过的操作记录，可以通过git reflog查看更加完整的历史纪录信息：
  ```
  $ git reflog
  0b26ea7 (HEAD -> temp, origin/temp) HEAD@{0}: commit: add linux
  75d416e HEAD@{1}: commit: start Hbase
  366be82 HEAD@{2}: commit: java redis
  c7c162a HEAD@{3}: commit: finish Redis 数据类型及其命令
  651d5b8 HEAD@{4}: Branch: renamed refs/heads/temp to refs/heads/temp
  651d5b8 HEAD@{6}: commit: Begin for week 2
  e3ca838 HEAD@{7}: pull origin temp: Fast-forward
  ea1e070 HEAD@{8}: commit: check for no-link
  7d03b6d HEAD@{9}: commit: finish week one
  11a0740 HEAD@{10}: checkout: moving from dev to temp
  ```
## 总结
总之，Git给我们提供了强大的功能，只要能找找到版本的commit_id,我们就可以通过`git reset --hard commit_id`指令回退到任何需要的版本。
