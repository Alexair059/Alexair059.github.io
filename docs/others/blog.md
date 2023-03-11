# 基于 mkdocs-material 的 Blog 工作流

晚上跟建筑系大神聊天，谈到如何搭建这个Blog的，于是记录一下自己摸索的过程，如果读者感兴趣也可以自己试试

## 初衷

跟大神聊天的时间，是愉快与轻松的，但也意识到不是所有人都对创建一个托管在Gitub Pages上的静态页面感兴趣。这既不是简历的加分项，也不是SRTP的创新点，更不是保研的绩点，甚至对于非计算机类的学生（例如建筑），大神说他们更倾向于去专门的网站与公众号获取业界相关的资讯

于是最后一个立意——用于向他人展示装逼，多多少少也被否决

但我很喜欢，因为我写字很慢，打字尚可。配合 latex 与 markdown ，我能一点点把自己在现实与网络的收获记录下来，这样在很久很久之后，我想拿点东西装逼时，好歹还有个这个

人生不能少了装逼，很多人对此有个高级说法，叫做实现人生价值

## 前置知识

- 基本的命令行操作
- git使用
- 任意代码编辑器的使用
- Github基本使用

## mkdocs-material 安装

具体参考官网Installation介绍

> https://squidfunk.github.io/mkdocs-material/getting-started/

## 创建仓库

在GitHub上创建一个repository

![](pre.assets/1.jpg)

在Repository name一栏写上 `<你的用户名>.github.io` ，此时它应该会提醒你自动启动了github-pages服务

## Git同步仓库并创建mkdocs项目

在本地进行git clone，并初始化项目，具体参考Creating your site介绍

> https://squidfunk.github.io/mkdocs-material/creating-your-site/

## 配置文件的设置

除开官网参考的设置

```shell
theme:
  name: material
```

有相当多的设置可以自行调节，下为个人Blog的 `mkdocs.yml` 部分界面设置：

```
ite_name: MZY Blog

theme:
  name: material
  language: zh
  logo: assets/logo.png
  favicon: assets/favicon.png
  palette: 
    - scheme: default
      primary: deep purple
      accent: deep purple
      toggle:
        name: Switch to light mode
        icon: material/brightness-7
    - scheme: slate
      primary: deep purple
      accent: deep purple
      toggle:
        name: Switch to dark mode
        icon: material/brightness-4
  
  features:
    - navigation.instant
    - navigation.tracking
    - navigation.tabs
    - navigation.top
    # - header.autohide

repo_url: https://github.com/Alexair059/Alexair059.github.io
repo_name: Alexair059.github.io
```

以及语法支持

```
markdown_extensions:
  # Python Markdown
  - abbr
  - admonition
  - attr_list
  - def_list
  - footnotes
  - md_in_html
  - toc:
      permalink: true

···

extra_javascript:
  - javascripts/mathjax.js
  - https://polyfill.io/v3/polyfill.min.js?features=es6
  - https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js
```

所有配置的解释都可以在官网查找

## 同步站点与项目

具体可参考官网Publishing your site介绍

> https://squidfunk.github.io/mkdocs-material/publishing-your-site/

在项目根目录输入

```shell
mkdocs gh-deploy
```

同步站点，再进行Git同步即可