---
title: Github pages & Hexo, 博客初体验
date: 2017-03-29 17:18:05
categories: 技能养成
tags: 新技能 博客搭建
---

博客已经搭起来一阵了，终于动手写这篇<小白搭建指南>了。
懒癌晚期患者实在是懒得折腾服务器来搭建博客，就越省事越好，所以选择了github pages。

##### 什么是github pages
[GitHub Pages](https://pages.github.com/) 本用于介绍托管在GitHub的项目，不过，由于他的空间免费稳定，用来做搭建一个博客再好不过了。
每个帐号只能有一个仓库来存放个人主页，而且仓库的名字必须是username/username.github.io，这是特殊的命名约定。你可以通过 http://username.github.io 来访问你的个人主页。
- 可以绑定你的域名(但暂时貌似只能绑定一个)。
- 使用Github Pages可以为你提供一个免费的服务器，免去了自己搭建服务器和写数据库的麻烦。

Tips : [个人感觉] 这个的好处就是项目可以完全留在本地，毕竟这个瞬息万变的时代指不定哪天别人的服务器就shut down了，那我辛辛苦苦码的字不就没了，所以自己保留一份还是比较稳妥的~

<!--more-->

##### 基础条件
此处假定你已经拥有了一下基础设施：
- Node.js环境  [官网](https://nodejs.org/en/download/)
- Git环境
- Github账号

##### 创建github仓库
1. 进入Github，点击导航栏的 + 号， 选择New repository
2. 创建repository，名称为你的用户名（比如我的是vonlyinno），其他选项自由选啦
3. 之后的访问域名就是[http://vonlyinno.github.io](http://vonlyinno.github.io/)
 
##### 配置SSH key
你提交代码肯定要拥有你的github权限才可以，但是直接使用用户名和密码太不安全了，所以我们使用ssh key来解决本地和服务器的连接问题。
具体请参考[官方教程](https://help.github.com/articles/connecting-to-github-with-ssh/)，介绍的还是比较完整的

##### Hexo简介
[Hexo](https://hexo.io/zh-cn/) 是一个快速、简洁且高效的博客框架。Hexo 使用 [Markdown](http://daringfireball.net/projects/markdown/)（或其他渲染引擎）解析文章，在几秒内，即可利用靓丽的主题生成静态网页。
以上是官网的介绍，其实就是自己用markdown写了文章，其他的布局、样式、分类、标签等一系列问题都交给Hexo就好了，我们只关心内容就ok。

##### 开始安装

>###### 安装之前先来说几个注意事项：
1. 很多命令既可以用Windows的cmd来完成，也可以使用git bash来完成，但是部分命令会有一些问题，为避免不必要的问题，建议全部使用git bash来执行；
2. hexo不同版本差别比较大，网上很多文章的配置信息都是基于2.x的，所以注意不要被误导；
3. hexo有2种_config.yml文件，一个是根目录下的全局的_config.yml，一个是各个theme下的；

###### 安装
```
npm install -g hexo-cli
```
安装好后，也通过查看版本，测试是否安装成功
```
hexo version
```
进入刚clone的hexo文件夹，初始化
```
cd hexo
hexo init
```
安装依赖
```
npm install
```
生成&启动，可以在http://localhost:4000/ 查看本地效果
```
hexo g  // 或者hexo generate
hexo s  // 或者hexo server，
```
安装可能出现的问题：
1. 执行hexo server提示找不到该指令
解决办法：
在Hexo 3.0 后server被单独出来了，需要安装server，安装的命令如下：
```
npm install hexo -server --save
```

安装好了，来看一下目录结构
```
├── .deploy       //需要部署的文件
├── node_modules  //Hexo插件
├── public        //生成的静态网页文件
├── scaffolds     //模板
├── source        //博客正文和其他源文件, 404 、favicon 、CNAME
|   ├── _drafts   //草稿
|   └── _posts    //文章
├── themes        //主题
├── _config.yml   //全局配置文件
└── package.json
```
###### [部署到github上](https://hexo.io/zh-cn/docs/deployment.html)
配置后就可以同步你的文章到github上了。配置deployer，根目录下的全局的_config.yml文件中，请确保每个deployer的缩进长度相同，并且使用空格缩进。
```
deploy:
  type: git
  repo: git@github.com:yourname/yourname.github.io.git
  branch: master
```
安装 hexo-deployer-git
```
npm install hexo-deployer-git --save
```
然后在当前目录打开命令行，输入：
```
hexo g
hexo d
```
或者
```
hexo g -d
```
执行完之后会让你输入github的账号和密码，输入完后就可以登录我们自己的部署在Github Pages服务器上的博客了。

###### [修改主题](https://hexo.io/zh-cn/docs/themes.html)
Hexo的主题确实让博客变美了很多，很多开发者也贡献了自己的主题，可以从[官网-主题](https://hexo.io/themes/)瞧一瞧看一看，此处也特别鸣谢一下我博客的[主题开发者](https://github.com/ppoffice)。

下载主题
```
git clone https://github.com/ppoffice/hexo-theme-icarus.git //主题的地址
```
配置全局站点文件_config.yml
```
themes: icarus //主题文件夹名
```
部署主题，查看效果
```
hexo g
hexo s
```
主题里有很多配置项，主题太多就不赘述了，还是去看主题文档吧。
###### [写文章](https://hexo.io/zh-cn/docs/writing.html)
这个很简单啊，下面的命令就会在 \hexo\source\_posts目录下创建一个myblog.md文件
```
hexo new myblog
```

###### [Hexo常用命令](https://hexo.io/zh-cn/docs/commands.html)
```
hexo help //查看帮助
hexo init //初始化目录
hexo new "postName" //新建文章
hexo new page "pageName" //新建页面
hexo generate //生成网页, 可以在public目录查看整个网站的文件
hexo server //本地预览,'Ctrl+C'关闭
hexo deploy //部署.deploy目录
hexo clean //清除缓存, 建议每次执行命令前先清理缓存
hexo n == hexo new
hexo g == hexo generate
hexo s == hexo server
hexo d == hexo deploy
```

##### 写在最后
我写markdown最近爱用[简书](http://www.jianshu.com)，可以实时保存+预览，对于md新手很友好，但是发现好像不能预览流程图。

最后附上[Hexo官方文档](https://hexo.io/zh-cn/docs/)，多读官方文档，有益身心健康。
See you.
