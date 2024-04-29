---
title: 使用hexo搭建博客网站，并使用butterfly主题
date: 2024-04-29 10:48:09
tags:
    - 建站
    - 博客
    - Hexo
    - Butterfly
cover: //cdn.jsdelivr.net/gh/donggangcj/CDN/image/blog-day1/hexo-gitpage.png
---

# 使用hexo搭建博客网站，并使用butterfly主题

## 1. 安装依赖

首先需要有[Node.js](http://nodejs.org/)和[Git](http://git-scm.com/)，这一点对于开发人员应该非常简单。

## 2. 安装Hexo

所有必备的应用程序安装完成后，即可使用 npm 安装 Hexo。

```shell
npm install -g hexo-cli
```

## 3. 建站

执行下列命令，Hexo 将会在指定文件夹中新建所需要的文件。

```shell
hexo init <folder>
cd <folder>
npm install
```

使用`server`子命令查看当前网站

```shell
hexo server
```

## 4. 部署到github

首先你需要一个[github](https://github.com/)账号，请在你的[ssh设置页面](https://github.com/settings/keys)里[配置ssh](https://docs.github.com/en/authentication/connecting-to-github-with-ssh)用于git链接github。

### 4.1 代码上传到github

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

## 5. 配置主题

```shell
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

### 新建`post`

```shell
hexo new post-name
```

### 启动本地服务

```shell
hexo server
```


### 推送到远程

```shell
git add .
git commit -m "somme comment"
git push
```

### 压缩仓库体积

由于`git`是增量更新的，按照道理来讲应该会越来越大，应该有方法用于删除之前的`commit`只保留当前的版本。

**这件事情留给需要的时候再做吧**
