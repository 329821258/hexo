---
title: 基于Hexo搭建个人博客
date: 2021年1月30日13:48:24
updated: 
type:	
comments: true
description: Butterfly主题博客搭建
keywords: KeyWords
top_img: https://pic1.zhimg.com/v2-41e1b825c51055f39c22b95777bc620b_1440w.jpg?source=172ae18b
mathjax: 
katex:
aside: 
aplayer:
highlight_shrink:
tags: Hexo
categories: Nodejs
cover: https://pic1.zhimg.com/v2-41e1b825c51055f39c22b95777bc620b_1440w.jpg?source=172ae18b
copyright_author: Bear
copyright_author_href: https://329821258.github.io/
copyright_url: 
copyright_info: 
---

## 1.安装Git

拉取蝴蝶模板要用到Git，先装好Git。

下载地址： https://git-scm.com/downloads 无脑下一步然后安装即可，装完后右键可以看到这两选项就是安装成功了

![image-20210130145430774](https://s3.ax1x.com/2021/01/30/ykm4e0.png)

## 2.安装Node.js 

Hexo框架是基于node.js生成的，所以安装Hexo之前要先装一下node.js

下载地址：http://nodejs.cn/download/ ，对照系统安装即可（无脑下一步），安装后配置下环境变量PATH（路径改成自己nodejs里面的）。

![image-20210130143024167](https://s3.ax1x.com/2021/01/30/ykmqSJ.png)

配完后命令行输入 node -v 和 npm -v 看到版本号就是安装成功了。

![image-20210130141322172](https://s3.ax1x.com/2021/01/30/ykmWyn.png)

## 3.安装Hexo

新建一个文件夹blog，然后进入该文件所在的命令行，安装hexo依赖包。

``` 
npm install -g hexo-cli
```

装完后输入hexo -v 看到版本号就表示安装成功了。

![image-20210130150218603](https://s3.ax1x.com/2021/01/30/ykm7YF.png)

成功后初输入以下命令始化hexo

``` 
hexo init hexo
```

初始化后进入hexo文件夹可以看到以下目录，这里我们只关心source这个文件夹 后续博客生成后都会放在这里![image-20210130150844608](https://s3.ax1x.com/2021/01/30/ykmLl9.png)

之后输入以下两条命令生成页面

``` 
hexo g && hexo s
```

![image-20210130151747973](https://s3.ax1x.com/2021/01/30/ykn3Xn.png)

然后进到 http://localhost:4000/   看到以下页面就可以了

![image-20210130152543489](https://s3.ax1x.com/2021/01/30/ykn676.png)

后续要写博客,可以在打开一个到hexo目录下的命令行然后输入以下命令（xxx是文件名）

``` 
hexo new xxx
```

就可以到hexo\source\\_posts看到生成 xxx.md文件（hello-world.md是默认自带的）

![image-20210130152315844](https://s3.ax1x.com/2021/01/30/yknahF.png)

生成完后回到页面就能看到刚刚生成的页面了

![image-20210130152543489](https://s3.ax1x.com/2021/01/30/yknbAf.png)

## 4.主题安装

这里我推荐一款我自己在用的hexo主题Butterfly

在hexo目录下打开命令行，输入以下命令拉取Butterfly主题文件

``` 
git clone -b master https://github.com/jerryc127/hexo-theme-butterfly.git themes/butterfly
```

![image-20210130153051496](https://s3.ax1x.com/2021/01/30/yku3CD.png)

修改_config.yml里面的配置，找到theme将主题指定为butterfly

``` yaml
# Extensions
## Plugins: https://hexo.io/plugins/
## Themes: https://hexo.io/themes/
theme: butterfly
```



然后在安装一个pug以及styles的依赖

``` 
npm install hexo-renderer-pug hexo-renderer-stylus --save
```

装完后重新运行 hexo g，hexo s命令 然后刷新一下就可以看到装好后的主题了

![image-20210130153729742](https://s3.ax1x.com/2021/01/30/ykudVP.png)

后续相应的配置更改可以看下 Butterfly 的教程  https://butterfly.js.org/posts/4aa8abbe/

## 5.发布到Gitee

没有账号的话，先到 https://gitee.com/ 上申请注册账号。

注册往后创建一个个人仓库路径和仓库名称随意

[![ye6BnS.png](https://s3.ax1x.com/2021/02/01/ye6BnS.png)](https://imgchr.com/i/ye6BnS)

填写后直接创建即可。然后回到个人主页找到刚刚创建的仓库，将仓库地址复制下来

[![yecwCR.png](https://s3.ax1x.com/2021/02/01/yecwCR.png)](https://imgchr.com/i/yecwCR)

然后回到hexo根目录 ，找到_config.yml文件，在底下配置gitee地址

``` yaml
# 把刚刚复制的地址粘贴到repo这里
deploy:
  type: git
  repo: https://gitee.com/xxxx/hexo.git		
  branch: master
```

打开命令行，安装自动部署工具，装完后重新发布（要输入gitee账号和密码）

``` 
npm install hexo-deployer-git --save 
hexo clean && hexo g && hexo deploy
```

回到仓库就可以看到生成的静态文件了。

[![yegdeS.png](https://s3.ax1x.com/2021/02/01/yegdeS.png)](https://imgchr.com/i/yegdeS)

接下来在项目的服务中选择Pages选项,部署pages时需要绑定手机号码. 点击启动.

[![ye2iOf.png](https://s3.ax1x.com/2021/02/01/ye2iOf.png)](https://imgchr.com/i/ye2iOf)

启动完后就可以访问上图的网站地址，这时候看到的页面可能格式是乱的，回到hexo根目录，找到_config.yml文件，找到url，将网站的粘贴到那，然后root指定仓库路径

``` 
url: https://xxx.gitee.io/hexo
root: /hexo
```

之后命令行重新发布,然后回到页面点一下更新，然后访问下网站地址就可以看到样式了

``` 
hexo clean && hexo g && hexo deploy
```

[![yeRuUe.png](https://s3.ax1x.com/2021/02/01/yeRuUe.png)](https://imgchr.com/i/yeRuUe)