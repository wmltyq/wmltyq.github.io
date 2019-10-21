---
title: 解决 Git 过程中的问题
date: 2018-07-07 17:05:14
tags: Git | GitHub
---

因为每次写完代码我都直接用 JetBrain 全家桶自带的 “Share Project on GitHub“ 将代码直接上传远程仓库，省去了写 .gitignore 的麻烦。有时候麻烦确实是省了不少，但是当我想做一些特定需求的事时，还是 Git 方便一点呀。

刚开始接触 GitHub 的时候我还年轻，不懂那么多。所以就遇到了下面一系列的问题：

* 不小心把 IDE 工具自动生成的文件 .idea 上传到了远程仓库，怎么删呀？

  1. 配置 .gitignore 文件

  ```
  echo ".idea" >> .gitignore
  ```

  2. 将 .gitignore 文件上传到远程仓库

  ```
  git add .gitignore
  git commit -m "edit .gitignore"
  git push origin master
  ```

  3. 删除 Git 的 .idea 文件

  ```
  git rm --cached -r .idea
  ```

  4. 同步到远程仓库

  ```
  git commit -m "delete .idea"
  git push origin master
  ```

* 在摸索中前行，在摸索中手贱：一不小心执行了 git rm -f *，一下子删除了 .gitignore 排除的所有文件，也就是我苦苦码出来的源代码，怎么找回呀？

  1. 通过 reset 命名找回。如果你删除的是特定文件，就将 * 号替换成你删除的文件即可，以下同理

  ```
  git reset HEAD *
  ```

  2. 查看状态

  ```
  git status
  ```

  3. 恢复文件

  ```
  git checkout -- *
  ```

* 等要提交代码的时候，发现远程仓库中有我当初通过浏览器界面创建的 README.md 文件，而本地代码目录中没有，怎么整呀？

  此时执行以下命名再提交代码即可

  ```
  git pull --rebase origin master
  ```

以上就是我在实际操作中遇到的一些关于 Git 操作的问题，其实大部分都能通过网络查找到相应的解决方案。在此梳理出来，一方面是想分享分享给大家，避免大家又采坑，另一方面是便于自己日后遇到同样的问题便于查询解决。