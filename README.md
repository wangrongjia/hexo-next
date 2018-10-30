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


