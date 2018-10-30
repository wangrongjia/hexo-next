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

设置npm阿里镜像
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
![hexo工程](https://wxpp.oss-cn-qingdao.aliyuncs.com/blogimages/%E5%88%A9%E7%94%A8github%20pages%E5%92%8Chexo%E6%90%AD%E5%BB%BA%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/hexo%E5%B7%A5%E7%A8%8B.png)







