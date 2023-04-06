---
password: mk4crypto
comments: true
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

在 `mkdocs.yml` 的 plugins 配置项中添加

```
- search: {}
  - encryptcontent: 
      title_prefix: '[限制]'
      summary: '[该内容已被加密保护]'
      encryption_info_message: '联系博主以获得权限'
      placeholder: 'Password'
      decryption_failure_message: '密码错误'
      # password_button: True
      # password_button_text: 'unlock'
      input_class: input-form
      # button_class: confirm-button
```

其中 input_class 配置项如果设置需自行引入CSS类，用以设定密码输入框样式，为此我们在 `docs/css` 下创建 `custom.css`

```css
.input-form {
    border: none;
    background-color: #f2f2f2;
    padding: 10px;
    border-radius: 5px;
    flex: 1;
}

.confirm-button {
    background-color: #4CAF50;
    border: none;
    color: white;
    padding: 10px;
    border-radius: 5px;
    cursor: pointer;
    height: 30px;
    margin: 10px 0 10px 20px;
    transition: background-color 0.3s;
}

.confirm-button:hover {
    background-color: #3e8e41;
  }
```

原本是同时设定了确认按钮的，但因为太丑就放弃了

同时在 `mkdocs.yml` 中添加额外 css 类

```
extra_css:
  - css/custom.css
```

