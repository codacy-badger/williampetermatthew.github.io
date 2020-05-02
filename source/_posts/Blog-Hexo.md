---
title: Hexo博客使用教程
date: 2019-03-02 08:11:18
tags: 
  - Hexo博客
---

- 为什么使用Hexo？
- 其实你想使用哪个使用哪个，只不过我个人觉得Hexo比较方便。
- 优点：灵活度高、主题丰富、插件丰富、资瓷Markdown和LaTeX。
- 缺点：要求有一定的计算机初阶知识水平，源码包含大量外文注释。

# 建站
## 软件
必不可少的有
- [Node.js](http://nodejs.cn/download/)（可以使用命令`node -v`查看版本）  
- ![Node.js](https://res.zhangkai.xin/pic/Blog-Hexo01.png)  
- [Git](https://git-scm.com/download/win)（可以使用命令'git version'查看版本）
- ![Git](https://res.zhangkai.xin/pic/Blog-Hexo02.png)    
这些是生成博客的必要软件

可选的有
- [Notepad++](https://notepad-plus-plus.org/download/)  
- ![Notepad++](https://res.zhangkai.xin/pic/Blog-Hexo04.png)  
这个是为了编写方便使用的软件，不推荐使用Windows自带的记事本和写字板编写文件。如果你手上有其他多功能文本编辑器，当然也可以使用。

> 注意：如果你选择使用Notepad++，应该在安装Git时将编辑器选择`Use Notepad++ as Git's default editor`。  
> ![DefaultEditor](https://res.zhangkai.xin/pic/Blog-Hexo03.png)  

## 安装Hexo
新建一个博客目录文件夹，然后右键文件夹中的空白处，选择`Git Bash Here`。
![](https://res.zhangkai.xin/pic/Blog-Hexo05.png)

等到出现以 `$ ` 开头的提示表示可以键入命令后，输入
```bash
$ npm i -g hexo
```
即可安装Hexo。

> 请注意：所有输入命令前的`$ `，只是为了表示是在Git Bash下执行的命令，实际输入时并不需要输入。

安装完后，输入
```bash
$ hexo init
```
等待初始化文件夹。初始化成功后，你会看到很多文件和文件夹

```
└Hexo				<-博客目录
 ├.deploy_git			<-上传博客的文件
 │└... 
 ├node_modules			<-插件目录（依赖包）
 │└...
 ├public			<-博客生成后的文件
 ├scaffolds			<-博客模板文件
 │├draft.md
 │├page.md
 │└post.md
 ├source			<-生成博客时的根目录
 │└_posts			<-博客目录
 │ └hello-world.md		<-初始示例博客文件
 ├themes			<-主题文件夹
 │└landscape			<-默认主题
 │ └...
 ├.gitignore			<-gitignore文件，用于git上传时使用
 ├_config.yml			<-博客主配置文件
 ├db.json			<-source解析文件
 ├package.json			<-项目所需模块项目的配置信息
 └package-lock.json		<-项目所需模块项目的配置信息
```

## 预览
输入
```bash
$ hexo server
```
> 如果提示不存在此指令，请输入以下指令安装
> ```bash
$ npm i hexo-server
```
然后打开浏览器，输入` http://localhost:4000 `，即可预览博客的样子。

如果要关闭，按 Ctrl+C 即可关闭。

> 如果你打开此地址看不到你的博客，存在一种情况是你安装的软件（例如福昕PDF阅读器）占用了你的4000端口，此时应该切换端口，例如5000。此时应该输入下方指令切换。
> ```bash
$ hexo server -p 5000
```

# 部署
## 安装插件
```bash
$ npm install hexo-deployer-git --save
```
## 传到平台
下面推荐几个平台
- [GitHub](https://github.com/)（强力推荐）
  - 优点：平台成立较早，设施完善，有微软收购的支持，全球的开发者均在此
  - 缺点：主语言为英文，且国内访问较慢。
- ~~[Coding](https://coding.net/)~~（已被腾讯收购，建议直接注册腾讯云开发者平台）
  - ~~优点：国内国外访问均快，设施较为完善。~~
  - ~~缺点：功能限制过大，服务器容易宕机~~
- [TencentDev](https://dev.tencent.com/)
  - 优点：腾讯服务器强大，国内国外访问均快
  - 缺点：腾讯功能限制过大
- [Gitee](https://gitee.com/)
  - 优点：与GitHub类似，但国内访问速度快，而且支持GitHub项目直接导入
  - 缺点：设施不太完善。

### 提前说明
在下文中：
- yourname指你注册网站的用户名
- youremail指你注册网站的邮箱

### GitHub
注册账号，然后创建一个repo，名字为`yourname.github.io`
在`_config.yml`里，编辑末尾的配置
```
deploy:
  type: git
  repo: git@github.com:yourname/yourname.github.io.git
  branch: master
```

在文件夹下键入命令
```bash
$ ssh-keygen -t rsa -C "youremail"
```
然后连敲几下回车，然后打开`C:\Users\你电脑的用户名`，找到`.ssh`文件夹，用编辑器打开`id_rsa.pub`文件，然后复制里面的所有文本。

在GitHub的Settings里，找到SSH keys，右上角新建一个SSH key，然后在Key那个大框中粘贴你复制的东西，随便起一个名字然后点下方的添加。

### Coding(TencentDev)
注册账号，然后创建一个repo，名字为`yourname`
在`_config.yml`里，编辑末尾的配置
```
deploy:
  type: git
  repo: git@git.coding.net:yourname/yourname.git
  branch: master
```

在文件夹下键入命令
```bash
$ ssh-keygen -t rsa -C "youremail"
```
然后连敲几下回车，然后打开`C:\Users\你电脑的用户名`，找到`.ssh`文件夹，用编辑器打开`id_rsa.pub`文件，然后复制里面的所有文本。

在网站的个人设置里，找到SSH keys，新建一个SSH key，然后在Key那个大框中粘贴你复制的东西，随便起一个名字然后点下方的添加。

### Gitee
注册账号，然后创建一个repo，名字为`yourname`
在`_config.yml`里，编辑末尾的配置
```
deploy:
  type: git
  repo: git@gitee.com:yourname/yourname.git
  branch: master
```

在文件夹下键入命令
```bash
$ ssh-keygen -t rsa -C "youremail"
```
然后连敲几下回车，然后打开`C:\Users\你电脑的用户名`，找到`.ssh`文件夹，用编辑器打开`id_rsa.pub`文件，然后复制里面的所有文本。

在Gitee的设置里，找到SSH keys，新建一个SSH key，然后在Key那个大框中粘贴你复制的东西，随便起一个名字然后点下方的添加。

### 多Git账户
生成多个key，即在执行命令
```bash
$ ssh-keygen -t rsa -C "youremail"
```
后的第一步将名字改为`id_rsa-github.pub`,`id_rsa-coding.pub`,`id_rsa-gitee.pub`，然后在`.ssh`文件夹下新建一个`config`文件。在文件内粘贴并修改以下内容
```
# 配置github.com
Host github.com                 
    HostName github.com         
    IdentityFile ~\\.ssh\\id_rsa-github
    PreferredAuthentications publickey
    User yourname-github

# 配置git.coding.net
Host git.coding.net             
    HostName git.coding.net     
    IdentityFile ~\\.ssh\\id_rsa-coding
    PreferredAuthentications publickey
    User yourname-coding

# 配置gitee.com                 
Host gitee.com                  
    HostName gitee.com
    IdentityFile ~\\.ssh\\id_rsa-gitee
    PreferredAuthentications publickey
    User yourname-gitee

```
即可

## 三步部署
按照上述配置好后，每次修改博客就只用执行下面三行指令就行了
```bash
$ hexo clean
$ hexo generate
$ hexo deploy
```

> 第一次执行之前需要执行下方两行指令设置个人信息
> ```bash
$ git config --global user.name "yourname"
$ git config --global user.email "youremail"
```
> 第一次执行时会询问你是否使用ssh（此时在弹出的框中输入yes即可）之类的操作
> 从第二次起就真的只用这三行了。

## 访问你的博客
### GitHub
在repo的Settings里向下翻到GitHub Pages，将 `None` 改为 `master branch` 。
然后就可以访问https://yourname.github.io/
### Coding(TencentDev)
在repo的代码下拉菜单点Pages服务，勾选`我已阅读《Coding Pages 服务声明》`，然后点击`一键开启 Coding Pages`。
然后就可以访问https://yourname.coding.me/  
### Gitee
在repo的服务下拉菜单点Gitee Pages，部署分支选择`master`，然后点击`启动`。
然后就可以访问https://yourname.gitee.io/  

# 设置博客
## 博客基本配置
你可以参照[Hexo官方文档](https://hexo.io/zh-cn/docs/)配置，也可以参照以下信息配置
```
# Site
title: 标题
subtitle: 副标题
description: 描述
keywords: 关键词
author: 作者
language: zh-CN
timezone: Asia/Shanghai

# URL
## If your site is put in a subdirectory, set url as 'http://yoursite.com/child' and root as '/child/'
url: 访问你博客的地址
root: /
permalink: :year/:month/:day/:title/
permalink_defaults:
```

## 写博客
你需要使用Markdown格式编辑博客  

你可以使用指令创建一篇写好基本信息的博客然后修改
```bash
$ hexo new 博客标题
```
也可以在_post文件夹下新建一个`.md`文件，并在开头写上
```
---
title: 文章标题
date: 20XX-XX-XX XX:XX:XX
tags: 
  - 标签
categories: 
  - 分类
---
```

然后在下面写正文部分。

> 当以上文字存在部分符号不合法时会出现错误，建议加上`""`号分割前后，例如：`title: "标题[1]"`
> 如果出现渲染错误，比如上面的内容出现在了文章开头而不是被隐藏，请检查文章的格式是否为Windows(CR LF)；
> 如果出现中文乱码，比如出现了 `˖Ԗ}ёڷޯ` 或 `鎵嬫寔涓ゆ妸妫嶆枻閾?` 这种乱码，请检查文章的编码是否为UTF-8；
> ![](https://res.zhangkai.xin/pic/Blog-Hexo06.png)

## 主题
有很多非常好的主题，由于主题数量庞大且编写风格不一，所以无法在这里为每位读者所喜爱的主题写建站教程。  
这里只介绍Vexo即我使用的主题的相关配置。

### 安装
大部分主题可以在GitHub上找到，找到后通过
```bash
$ git clone https://github.com/作者/hexo-theme-主题.git themes/主题
```
这条指令下载到主题文件夹中，然后在博客目录下的`_config.yml`中写上
```
theme: 主题
```
### Vexo
作者：[yanm1ng](https://github.com/yanm1ng)  
主题地址：[hexo-theme-vexo](https://github.com/yanm1ng/hexo-theme-vexo)

#### 安装
可以通过
```bash
$ git clone https://github.com/yanm1ng/hexo-theme-vexo.git themes/vexo
$ cp -R themes/vexo/_source/* source/
```

同时修改主目录下的`_config.yml`
```
theme: vexo
```
完成初步的安装

#### 文章
事实上，在每篇文章开头的*front-matter*都这么写。
```
---
title: 标题
date: 20XX-XX-XX XX:XX:XX
banner: 图片.扩展名
tags:
 - 标签1
 - 标签2
---
```
#### 配置
在主题的`_config.yml`里，有这些东西，可以看我的注释配置你的
```
author: 作者
description: 描述
keyword: 关键词
favicon:      #图标，默认没有

excerpt_link: Read more  #查看更多的提示文字

menu:
 主页: /
 标签: /tags/
 归档: /archives/
 项目: /project/
 关于: /about/
```

然后翻到最下面
```
about: 
  banner: 图片.扩展名
  avatar: 头像地址
  description: 描述
  weibo_username: 微博用户名
  twitter_username: Twitter用户名
  github_username: GitHub用户名
  zhihu_username: 知乎用户名
  douban_username: 豆瓣用户名
  linkedin_username: 领英用户名
# 没有的话不填就行了
```
#### 自定义图片
打开主题下的`/source/css/images`文件夹，**替换** `logo.png` 、 `alipay.jpg` 和 'wechat.jpg' 。


# 插件
## 评论系统
有很多很优秀的评论插件，例如Gitment、Gitalk、Valine、Disqus、Uyan等。  
由于本人使用的是Gitment，所以只会介绍Gitment。。。  
> 建议直接使用主题所附带的评论系统，这样一般是兼容页面的既好用又好看。

### Vexo下的Gitment
> Gitment利用GitHub上repo中的Issues功能完成评论存储的功能。  
> 注意，Gitment只支持GitHub上的repo，如果你要利用Coding或者Gitee的功能，可以尝试DIY。

请[点这里](https://github.com/settings/applications/new)打开GitHub的OAuth应用创建页面，输入博客名字、主页地址、描述和调用地址。**请注意：主页地址和调用地址都要填你的博客的地址。**  
![](https://res.zhangkai.xin/pic/Blog-Hexo07.png)

创建完后，复制Client ID和Client Secret的信息。

请点开Vexo主题下的`_config.yml'，将下面对应的行修改成这样
```
comment: gitment
```

然后在下方对应的部分
```
# gitment config
gitment_owner: yourname
gitment_repo: 博客地址
gitment_oauth_id: 复制的Client ID
gitment_oauth_secret: 复制的Client Secret
```
即可完成配置

> 请注意，每篇博客都需要博主手动初始化，否则会显示Error。  
> ![](https://res.zhangkai.xin/pic/Blog-Hexo08.png)  
> ![](https://res.zhangkai.xin/pic/Blog-Hexo09.png)  

## 置顶操作
打开博客目录下的`.\node_modules\hexo-generator-index\lib`文件夹，将`generator.js`文件内容替换为下面的代码
```javascript
'use strict';
var pagination = require('hexo-pagination');
module.exports = function(locals){
  var config = this.config;
  var posts = locals.posts;
    posts.data = posts.data.sort(function(a, b) {
        if(a.top && b.top) { // 两篇文章top都有定义
            if(a.top == b.top) return b.date - a.date; // 若top值一样则按照文章日期降序排
            else return b.top - a.top; // 否则按照top值降序排
        }
        else if(a.top && !b.top) { // 以下是只有一篇文章top有定义，那么将有top的排在前面（这里用异或操作居然不行233）
            return -1;
        }
        else if(!a.top && b.top) {
            return 1;
        }
        else return b.date - a.date; // 都没定义按照文章日期降序排
    });
  var paginationDir = config.pagination_dir || 'page';
  return pagination('', posts, {
    perPage: config.index_generator.per_page,
    layout: ['index', 'archive'],
    format: paginationDir + '/%d/',
    data: {
      __index: true
    }
  });
};
```

在博客开头的部分定义top值
```
---
top: 数值
---
```

> 说明：top没填时默认为0，置顶顺序为top值降序排列，top相等时按日期排序。

# 自动部署
自动部署需要一个网站——Travis CI，这个网站分为 [PublicRepo版](https://travis-ci.org/) 和 [PrivateRepo版](https://travis-ci.com/)，但是只支持GitHub上的项目。  
> 对于自动部署博客，我的做法需要在上传到GitHub的文件里写上你Github的密码，因此强烈建议各位新建一个Private项目。

我们新一个项目（当然也可以在原repo上新建一个分支），然后[点这里](https://github.com/settings/tokens)生成一个AccessToken（建议把除了delete_repo以外的权限全部开启），然后把出来的一串值复制。

打开Travis CI，点开你新建的项目，在Environment Variables里，在Value处粘贴那串值，名字随意，然后点击Add。
![](https://res.zhangkai.xin/pic/Blog-Hexo10.png)

返回Hexo的文件目录，执行
```bash
$ git init
$ git remote add origin https://github.com/yourname/repo的名字.git
```
使用命令行新建一个`.travis.yml`文件
```batch
cd.>.travis.yml
```
![](https://res.zhangkai.xin/pic/Blog-Hexo11.png)

粘贴如下内容并修改后保存
```
language: node_js
node_js: stable

# S: Build Lifecycle
install:
  - npm install

#before_script:
 # - npm install -g gulp

script:
  - hexo g

after_script:
  - cd ./public
  - git init
  - git config user.name "yourname"
  - git config user.email "youremail"
  - git add .
  - git commit -m "Site Updated"
  - git push --force --quiet "https://${GH_TOKEN}@${GH_REF}" master:master
# E: Build LifeCycle

branches:
  only:
    - master
env:
 global:
   - GH_REF: github.com/yourname/repo的名字.git
   - GH_TOKEN: yourname:GitHub密码
```

这样处理好后执行以下命令
```bash
$ git add .
$ git commit -m "Site Updated"
$ git push origin master
```
当显示完成时，打开Travis，如果显示build passing，则说明正常（如果网站还未更新、显示build failing等，可以检查下配置）。
> 状态：
> ![passing](https://res.zhangkai.xin/pic/passing.svg)
> ![failing](https://res.zhangkai.xin/pic/failing.svg)
> ![error](https://res.zhangkai.xin/pic/error.svg)
> ![unknown](https://res.zhangkai.xin/pic/unknown.svg)

这样就配置好了。

# 补充说明
在安装时如果没有选中notepad++，也可以在后来执行以下命令改为notepad++
```bash
$ git config --global core.editor notepad++
```

hexo的很多命令都可以简写
比如
- `hexo server`		->	`hexo s`
- `hexo generate`	->	`hexo g`
- `hexo deploy`		->	`hexo d`

但是由于c开头的指令不止一个，所以clean无法简写为c。

使用Gitment时如果想使用Coding、Gitee等地方的Pages，可以考虑只使用GitHub的repo当一个评论仓库，就可以利用其它平台的Pages了。

自动部署系统Travis CI虽然只支持GitHub，但是我们可以只利用GitHub作源码仓库，把Pages的仓库利用其它平台存储，做法是修改`.travis.yml`中的最后的git指令。

执行时如果出现错误：fatal: remote origin already exists，则执行以下语句：
```bash
$ git remote rm origin
```
再执行
```bash
$ git remote add origin https://github.com/yourname/repo的名字.git
```

如果出现错误failed to push som refs to…….，则执行以下语句，先把远程服务器github上面的文件拉先来，再push 上去：
```bash
$ git pull origin master
```

npm切换为淘宝源
```bash
$ npm install -g cnpm --registry=https://registry.npm.taobao.org
$ npm config set registry https://registry.npm.taobao.org
```


------------

> [![知识共享许可协议](https://res.zhangkai.xin/pic/license/BY-NC-SA_80x15.png)](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh)
> 
> [![知识共享许可协议](https://res.zhangkai.xin/pic/license/BY-NC-SA_88x31.png)](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh)
> 
> 本作品采用[知识共享署名-非商业性使用-相同方式共享 4.0 国际许可协议](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh)进行许可。