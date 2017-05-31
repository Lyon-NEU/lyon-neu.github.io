---
layout: post
title: "jekyll+github pages搭建博客"
description: ""
category: 
tags: []
---
{% include JB/setup %}


### 创建自己的Github pages 

* 注册自己的`*github*`帐号
* 新建`new repository`,名字命名为username.github.io
* 进入此项目，点击`setting`,选择autimatic generate layout ,然后继续选择布局,保存
* 访问地址 http://username.github.io

### 安装Jekyll

`Win10 64`位环境下的安装过程

* 安装`Ruby`
    - 下载[RubyInstaller](http://www.ruby-lang.org/en/documentation/installation/)运行安装

* 安装`RubyGems`
    - [下载](https://rubygems.org/pages/download)
    - 解压压缩包，进入目录
    - 运行 `ruby setup.rb`
    
* 安装jekyll
* `gem install jekyll`

### 下载Bootstrap模板

    ```
    git clone https://github.com/plusjade/jekyll-bootstrap.git USERNAME.github.com
  	cd USERNAME.github.com
  	git remote set-url origin git@github.com:USERNAME/	USERNAME.github.com.git
  	git push origin master
    ```
 [参考themes](http://jekyllbootstrap.com/usage/jekyll-theming.html)

### 发布到Github

- 在git新建一个仓库，如`my-jyky`
- git remote set-url origin https://github.com/usernmae/`my-jeky`.git
- git add .
- git commit -m ''
- git push origin master
- git checkout -b gh-pages  
- 修改根目录下地_config.yml文件 <br>

    > `production_url` : http://lyon-neu.github.io <br>
    > `JB`：<br>
    > 
    >> `BASE_PATH` : /my-jeky

查看bootstrap模板下的/asset/themes/目录，可以看到有bootstrap-3和twitter两个文件夹，实际布署的时候会提示找不到bootstrap/，所次将bootstrap-3里的文件全部复制到/themes/目录下

## To do
* table不能解析
在实际应用中，不知道什么原因，尝试多次，jekyll始终不能渲染表格


[jekyll-en](https://jekyllrb.com/)<br>
[jekyll-中文](http://jekyllcn.com/docs/home/) <br>
[Setting up your Github Pages site locally with Jekyll](https://help.github.com/articles/setting-up-your-github-pages-site-locally-with-jekyll/)  

