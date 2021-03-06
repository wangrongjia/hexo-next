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

资源文件夹(source)是存放用户资源的地方。除 _posts 文件夹之外，开头命名为 _ (下划线)的文件 / 文件夹和隐藏的文件将会被忽略。Markdown 和 HTML 文件会被解析并放到 public 文件夹，而其他文件会被拷贝过去。


`生成静态文件`
```
hexo generate
```

多出public文件夹

启动hexo服务
```
hexo server
```

访问 [localhost:4000](localhost:40000)

可以看到hexo默认生成的页面
![hexo默认生成的页面](https://wxpp.oss-cn-qingdao.aliyuncs.com/blogimages/%E5%88%A9%E7%94%A8github%20pages%E5%92%8Chexo%E6%90%AD%E5%BB%BA%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/hexo%E9%BB%98%E8%AE%A4%E7%94%9F%E6%88%90%E7%9A%84%E4%B8%BB%E9%A1%B5.png)

其中，已经默认写了一篇`布局`为`post`的文章 `hello-hexo.md`

hexo默认的主题是landscape,可以看到,效果不是很好,我们可以在[hexo主题](https://hexo.io/themes/)中选择自己喜欢的主题,并替换之,我用的是[next](https://github.com/theme-next/hexo-theme-next),在themes文件夹下git clone一份,并修改`_config.xml` 不需要重启hexo

接下来我们学习怎么生成

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
language: zh-CN
timezone: Asia/Shanghai

# URL
url: http://www.codinger.com.cn
root: /

# Writing
## 选择默认布局(post表示发布，执行 hexo new [layout] <title> 指令时默认的布局，draft表示草稿)
default_layout: draft

## Themes: https://hexo.io/themes/
## 默认的theme是landscape，我们修改成next
theme: hexo-theme-next

# Deployment
# 配置github的部署路径为自己的github主页，部署的时候会自动上传到githubIO上
deploy:
  type: git
  repo: https://github.com/wxpp/wxpp.github.io.git
  branch: master
```
scaffold 
布局的模板


三种基本布局

布局	| 含义 | 模板 | 路径 
--- | --- | --- | ---
post | 发布的文章 | post.md  |	source\_posts
page	| 自定义的页面 | page.md  | source
draft	 | 草稿箱 | draft.md | source\_drafts

下面演示新建一个草稿,然后发布它到文章中

```
hexo new draft test-draft
```
 
在`source -> _drafts` 路径下生成 `test-draft.md`文件

我们添加一些内容,然后发布到 `_post`路径下

```
hexo publish draft test-draft
```

那么它就从 草稿箱发布到 发布文章中了

`hexo generate` 就可以看到新的文章了

page这个模板,可以生成新的页面

```
hexo new page newPage
```

`source` 目录下多出 `newPage` 文件夹,里面含有`index.md` ,编辑它,然后在next主题的配置文件(也叫`_config.xml`,本文会用 `theme_config.xml`来区分)中添加
```yaml
menu:
  home: / || home
  about: /about/ || user
  tags: /tags/ || tags
  categories: /categories/ || th
  archives: /archives/ || archive
  newPage: /newPage || th
  
# Schemes 选择主题模式
#scheme: Muse
scheme: Mist
#scheme: Pisces
#scheme: Gemini  

social:
  GitHub: https://github.com/wxpp || github
```

这样就可以多出一个 `newPage` 的 `分页`
![next主题分页](https://wxpp.oss-cn-qingdao.aliyuncs.com/githubImages/hexo-next/readme.md/hexo-pages.PNG)

要开启`categories`和`tags` 需要这样的操作

然后将 `categories`和`tags` 文件夹下的 `index.md` 中分别添加 `types: categories` 和 `types: tags`

## next主题集成gitalk

首先,我们需要在github上申请一个`OAuth App` settings->developer settings -> new OAuth App

其中 `Homepage URL`填写github.io的地址  例如,我的是 `https://wxpp.github.io`

`Authorization callback URL` 如果githubIO没有绑定自己的域名 ,也填写github.io的地址,否则填写自己的域名  例如,我的是 `https://www.codinger.com.cn`

接下来,在 `theme_config.xml`中打开gitalk

```yaml
gitalk:
  enable: true
  github_id:  wxpp # Github repo owner
  repo:  wxpp.github.io # Repository name to store issues.
  client_id:  xxxxx # Github Application Client ID
  client_secret:  xxxxxx # Github Application Client Secret
  admin_user:  wxpp # GitHub repo owner and collaborators, only these guys can initialize github issues
  distraction_free_mode: true # Facebook-like distraction free mode
```

这样,就添加好了,但是每篇文章需要登录github账号后 ,在网站中点进去,这样才会开启评论














