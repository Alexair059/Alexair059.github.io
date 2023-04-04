---
password: mk4crypto
---

# mkdocs-encryptcontent-plugin：相当简单的mkdocs加密插件

实话说这是一次意外之喜，一开始只是想了解一下基于mkdocs框架的静态博客有没有现成的网页加密解决方案

如果能够实现，也算间接满足了自己的中二小愿望吧，一个自己掌握着 Password 的网页，就好像小时候有一个自己的秘密基地一样，哪怕它只是老家转角楼梯底下的杂物格

这一次学习的过程对ChatGPT在工程方面的作用有了全新的认识，确实是对自学者很大的助力

## 来源与安装

> https://pypi.org/project/mkdocs-encryptcontent-plugin/

mkdocs-encryptcontent-plugin 插件的安装相当简单

```shell
pip install mkdocs-encryptcontent-plugin
```

并在 mkdocs 的配置文件 `mkdocs.yml` 启用

```
plugins:
    - search: {}
    - encryptcontent: {}
```

## 基本设置

Todo

