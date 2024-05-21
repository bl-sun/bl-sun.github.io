---
title: 使用hexo搭建博客网站，并使用butterfly主题
date: 2024-04-29 10:48:09
updated: 2024-05-21 15:57:00
tags:
    - 建站
    - 博客
    - Hexo
    - Butterfly
categories:
   - [建站]
cover: //cdn.jsdelivr.net/gh/donggangcj/CDN/image/blog-day1/hexo-gitpage.png
---

# 使用hexo搭建博客网站，并使用butterfly主题

## 1. 安装依赖

首先需要有[Node.js](http://nodejs.org/)和[Git](http://git-scm.com/)，这一点对于开发人员应该非常简单。

> 推荐使用`NVM for Windows`安装`nodejs`

## 2. 建站

创建一个空文件夹，在文件夹内执行下列命令，Hexo将会在该文件夹中新建所需要的文件。

```shell
npx hexo init
```

> 第一次执行时可能较慢，npx会临时安装hexo包，并在运行结束后删除hexo包。这样无需对hexo进行全局安装。
>
> 但是`hexo init`会在文件夹中局部安装hexo。并安装所依赖的包。因此也无需执行`npm install`

使用`server`子命令查看当前网站

```shell
npx hexo server
```

## 3. 部署到github

首先你需要一个[github](https://github.com/)账号，请在你的[ssh设置页面](https://github.com/settings/keys)里[配置ssh](https://docs.github.com/en/authentication/connecting-to-github-with-ssh)用于git链接github。

### 3.1 代码上传到github

**step 1. 建立名为`<你的 GitHub 用户名>.github.io`的储存库**

**step 2. 在储存库中前往`Settings > Pages > Source`，并将`Source`改为`GitHub Actions`**

**step 3. 配置`GitHub Actions`配置文件**

使用`node --version`指令检查你电脑上的`Node.js`版本，并记下该版本(例如：`v20.y.z`)

建立`.github/workflows/pages.yml`，并填入以下内容(将`20`替换为上个步骤中记下的版本)

**step 4. 初始化本地仓库并部署到github**

> 如果你第一次使用git可能需要配置用户名和邮箱

```bash
git init
git add .
git commit -m "first commit"
git branch -M main
git remote add origin git@github.com:<你的 GitHub 用户名>/<你的 GitHub 用户名>.github.io.git
git push -u origin main
```

> 默认情况下`public/`不会被上传(也不该被上传)，确保`.gitignore`文件中包含一行`public/`。整体文件夹结构应该与[示例储存库](https://github.com/hexojs/hexo-starter)大致相似。

## 4. 配置主题

```shell
npm uninstall hexo-theme-landscape
git submodule add -b master https://github.com/jerryc127/hexo-theme-butterfly.git themes/butterfly
```

修改`Hexo`根目录下的`_config.yml`，把主题改为`butterfly`

```yml
theme: butterfly
```

将`/themes/butterfly/_config.yml`复制到`/_config.butterfly.yml`

安装插件

```shell
npm install hexo-renderer-pug hexo-renderer-stylus --save
```

其他配置请参照[https://butterfly.js.org/posts/dc584b87/](https://butterfly.js.org/posts/dc584b87/)

将配置或修改推送到远程仓库

```shell
git add .
git commit -m "somme comment"
git push
```

## 自此以后

以上配置基本只操作一次，以后就可以开始愉快的写文章啦！

### 写博客

一般情况下我们会开启一个hexo server然后新建一个post或者打开之前的post。这样可以一边修改一边查看页面的变化。（需要手动刷新）

#### 1 新建`post`

```shell
npx hexo new post-name
```

#### 2 启动本地服务

```shell
npx hexo clean
npx hexo server
```


### 推送到远程

```shell
git add .
git commit -m "somme comment"
git push
```

对博客所做的所有更改无论是配置或者添加新的文章都可以通过推送代码，快速部署。

### 重新搭建博客环境

当我们换了另一台设备时，可能需要重新搭建博客环境

#### 1 安装git和nodejs

#### 2 从代码仓库克隆源代码

```shell
git clone --recursive <repo>
```

#### 3 安装博客依赖

```shell
npm install
```

#### 4 尝试启动博客

```shell
npx hexo server
```

### 压缩仓库体积

由于`git`是增量更新的，按照道理来讲应该会越来越大，应该有方法用于删除之前的`commit`只保留当前的版本。

**这件事情留给需要的时候再做吧**
