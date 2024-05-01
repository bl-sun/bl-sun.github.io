---
title: git 学习笔记
date: 2024-04-28 21:10:18
categories:
   - git
tags: 
   - git
   - 编程
cover: //tse2-mm.cn.bing.net/th/id/OIP-C.N2A89Kvz4PMLfkYBBqgcOAHaEK?rs=1&pid=ImgDetMain
---

# git 学习笔记

## 1. 工作流

【十分钟学会正确的github工作流，和开源作者们使用同一套流程】 [https://www.bilibili.com/video/BV19e4y1q7JJ/](https://www.bilibili.com/video/BV19e4y1q7JJ/?share_source=copy_web&vd_source=6be7dc3ae8a003e840c89d32520c84b0)

<iframe style="width: 100%; aspect-ratio: 16/9;" src="//player.bilibili.com/player.html?bvid=BV19e4y1q7JJ&poster=1&autoplay=0" frameborder="no" scrolling="no"></iframe>

1. 克隆到本地并开发
   1. `git clone https://gitee.com/lei2019/mongolian_news_crawler.git` # 到本地
   2. `git checkout -b xxx` 切换至新分支xxx（相当于复制了remote的仓库到本地的xxx分支上
   3. 修改或者添加本地代码（部署在硬盘的源文件上）
   4. `git diff` 查看自己对代码做出的改变
   5. `git add` 上传更新后的代码至暂存区
   6. `git commit` 可以将暂存区里更新后的代码更新到本地git
   7. `git push origin xxx` 将本地的xxxgit分支上传至github上的git
2. 开发完成后分支合并（如果在写自己的代码过程中发现远端GitHub上代码出现改变）
   1. `git checkout main` 切换回main分支
   2. `git pull origin master`(main) 将远端修改过的代码再更新到本地
   3. `git checkout xxx` 回到xxx分支
   4. `git rebase main` 我在xxx分支上，先把main移过来，然后根据我的commit来修改成新的内容
   5. （中途可能会出现，rebase conflict -----》手动选择保留哪段代码）
   6. `git push -f origin xxx` 把rebase后并且更新过的代码再push到远端github上（-f ---》强行）
   7. 原项目主人采用pull request 中的 `squash and merge` 合并所有不同的commit
3. 远端完成更新后
   1. `git branch -d xxx` 删除本地的git分支
   2. `git pull origin master` 再把远端的最新代码拉至本地
   3. `git remote prune origin` 同步“修剪”分支，保持分支的一致性

```bash
# 1. 克隆到本地并开发相应的功能
git clone <repo>
git checkout -b feat-xxx
# 编写代码
git diff
git add <pathspec>...
git commit
git push origin feat-xxx

# 2. 开发完成后分支合并
git checkout main
git pull origin main
git checkout feat-xxx
git rebase main
# 可能会出现，rebase conflict。手动选择保留哪段代码
git push -f origin feat-xxx
# 原项目主人采用pull request 中的 `squash and merge` 合并所有不同的commit

# 3. 远端完成更新后
git branch -d feat-xxx
git pull origin main
git remote prune origin
```

### 单分支工作流

1. 初始化仓库
   1. `git clone url`或者`git init`
2. 本地开发
   1. 修改、编写代码 - 开发相应的功能
   2. `git diff` 查看自己对代码做出的改变
   3. `git add .`
   4. `git commit -m "comment"`
   5. 可选择重复进行步骤2.1开发相应的功能，后add commit
3. 将本地修改推送到远程仓库
   1. `git push`
   2. push出错 - 如果有人在main分支上做了修改，那么push就会出错
      1. `git pull`或`git pull --rebase`
      2. 可能产生冲突 - 当你与远程仓库上最新的提交修改了同一个文件，那么就可能出现冲突
         1. 处理冲突 - 使用`git status`查看存在冲突的文件 - 选择要保留的代码
         2. `git add .; git commit -m "comment"`冲突处理完毕后提交代码
      3. 冲突处理完毕再次进行步骤 3.1 `git push` push的时候仍然可能会出错
4. 接下来可以再进行步骤2本地开发

## 2. git学习工具

- 猴子都能懂的GIT入门 | 贝格乐（Backlog） | [https://backlog.com/git-tutorial/cn/](https://backlog.com/git-tutorial/cn/)
- git 分支 闯关 | Learn Git Branching | [https://learngitbranching.js.org/](https://learngitbranching.js.org/)
- 廖雪峰的官方网站 | [https://www.liaoxuefeng.com/](https://www.liaoxuefeng.com/)

【团队开发神器 Git/GitHub 自学指南，几分钟掌握学习重点】 [https://www.bilibili.com/video/BV1KZ4y1e7cG/](https://www.bilibili.com/video/BV1KZ4y1e7cG/?share_source=copy_web&vd_source=6be7dc3ae8a003e840c89d32520c84b0)

<iframe style="width: 100%; aspect-ratio: 16/9;" src="//player.bilibili.com/player.html?bvid=BV1KZ4y1e7cG&poster=1&autoplay=0" frameborder="no" scrolling="no"></iframe>

## 3. 子模块 submodule

### 知识1：克隆一个带有submodule的库

#### 方法1：

```shell
git clone <url> --recursive
```

#### 方法2：

```shell
git clone <url>
```

**下载所有submodule**

```shell
git submodule init
git submodule update --recursive
```

**下载指定的submodule**

```shell
git submodule init lib1 lib2
git submodule update # 会update所有被init过的submodule
```

### 知识2：在现有仓库中添加submodule

```shell
git submodule add <url> <dir>
```

### 知识3：更新submodule

#### 方法1：

```shell
cd <submodule>
git pull
```

#### 方法2：

```shell
git submodule foreach git checkout master
git submodule foreach git pull
```

```shell
git submodule update --remote
```

### 知识4：删除现有仓库中的submodule

## 4. subtree

github中还是submodule用的多一点，等有时间再深入研究
