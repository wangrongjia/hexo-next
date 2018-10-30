# 利用github pages和hexo搭建个人博客

## 1. github pages

什么是github pages?[GitHub Pages](https://pages.github.com/)

新建一个 userName.github.io的仓库，比如我的是[wxpp.github.io](https://wxpp.github.io)

初始化该仓库后，Settings选项中找到github Pages,其中的source如果是none,选择一个分支，保存后,就开启了自己的个人主页：https://yourName.github.io

如果需要绑定域名，需要`域名解析`,`githubio添加CNAME文件`

域名解析

记录类型 |	主机记录	|	记录值  
--- | --- | ---   
A	 |  @	 |	192.30.252.153
CNAME	| www	|	wxpp.github.io

其中ip地址可以通过ping yourName.github.io 得到

仓库中添加CNAME文件,其中填写自己的域名
`CNAME`
```CNAME
www.codinger.com.cn
```

这样，就可以通过你的域名访问github的个人主页

以上，我们的github才只能展示自己的readme文件，怎么展示更多的东西呢？选择合适的静态博客框架，当前主流的静态博客框架有[jekyll](http://jekyllcn.com/)和[hexo](https://hexo.io/zh-cn/),本文简单介绍一下怎么使用next搭建博客

## 2. hexo

[hexo官方文档](https://hexo.io/zh-cn/docs/)

我的搭建步骤：
首先，使用hexo需要本机安装node.js和git的环境

然后，设置npm阿里镜像
```
npm config set registry https://registry.npm.taobao.org
```
安装hexo-cli
```
$ npm install -g hexo-cli
```
生成hexo工程
```
$ hexo init <folder>
$ cd <folder>
$ npm install
```

生成的目录如下
```
.
├── _config.yml
├── package.json
├── scaffolds
├── source
|   ├── _drafts
|   └── _posts
└── themes
```

其中最重要的配置文件_config.xml

其中一些需要修改的参数
`_config.xml`
```xml
# Site
title: 扣腚鳄
subtitle: www.codinger.com.cn
description: 本站是扣腚鳄的技术分享博客。内容涵盖生活故事、Java后端技术、Spring Boot、Spring Cloud、微服务架构等相关的研究与知识分享。
keywords: codinger,扣腚鳄,Spring,Spring Boot,Spring Cloud,MongoDB,Jvm,生活故事,架构,开发者,编程,代码,开源,IT网站,Developer,Programmer,Coder,Geek,IT技术博客,Java,
author: codinger
language: zh-cn
timezone: Asia/Shanghai

# URL
url: http://www.codinger.com.cn
root: /

# Writing
## 选择默认布局(post表示发布，执行 hexo new [layout] <title> 指令时默认的布局，draft表示草稿)
default_layout: draft

## Themes: https://hexo.io/themes/
## 默认的theme是landscape，我们修改成next
theme: next

# Deployment
# 配置github的部署路径为自己的github主页，部署的时候会自动上传到githubIO上
deploy:
  type: git
  repo: https://github.com/wxpp/wxpp.github.io.git
  branch: master
```
scaffold 
布局的模板

三种布局对应的路径
布局	| 路径
--- | ---
post |	source/_posts
page	| source
draft	 | source/_drafts







