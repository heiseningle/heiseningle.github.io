---
layout:     post
title:      通过github-pages搭建blog
subtitle: 
date:       2017-11-18
author:     zz
header-img: img/post-bg-ios9-web.jpg
catalog: true
tags:
    - blog 
    - tech
    - meta
---


## 历程
从2016年末开始思考自己的meta能力、职业方向和擅长。当时就想着要搭建一个自己的blog:
1. 一方面为了记录自己的想法和总结  
2. 另一方面作为找工作时候的展示
可是因为年初进入业务线负责产品渠道，再接下来遇到架构调整、晋升等等事情，没有时间并且寻找新机会的步子就放缓了，基本就只在evernote上进行想法记录。

于是在2017年11月，时隔一年之后重新来看怎么搭建一个blog。
过程当中还是遇到了年初时候的坑，还出了更多的问题。在年初了解的基础上，又总共花费了两天的时间才将github、git、jekyll、bundler、rvm、ruby&gem、homebrew等相关概念搞清楚，并前置学习了macOS（linux）的文件结构，才最后将blog搭建起来（还是用了别人的模板，加上前端代码学习估计要一个月）。

在本post里面，对搭建之路遇到的问题逐一进行反思和解决方案记录。此外，通过这次搭建blog对学习方法做了精进（正好是2016年末的时候开始思考自身的元能力建设，历时一年后可以应用到实践当中并总结）。



## 概念梳理
1. **Rails**
- 是Ruby语言比较成熟的一个框架，使用它可以快速将MVC三层搭好 

2. **bundler**
-  a gem to bundle gems make sure every ruby applications run the same code on each machine.
-  bundler能做到这一点是因为不但安装用户指定的Gems，并且将这些Gems相关的所有以来gems都安装了。并且，在安装之前会检查每一个gem的可用性和版本，确保是能够同时装载的，并对版本进行记录。由此，保证了所有用户都是在用同一套code
- 常用bundle exec：所有依赖一起加载
- bundler: Bundler maintains a consistent environment for ruby applications. It tracks an application’s code and the gems it needs to run, so that an application will always have the exact gems (and versions) that it needs to run. 这是一个ruby程序，作用是根据gemfile声明的source和gem下载所有相关的gem。关于bundler： http://xiajian.github.io/2014/10/22/bundle

3. **Gemfile**
- 是一个什么概念：是一个文本文件，里面标注了source，指向下载网站；接着指明了需要的包；bundler从gemfile了解要下载的包，进而将所有denpendencies下载下来
- 每次运行bundle install后，会对本文件目录下的Gemfile进行解析，下载所有列示的gem和dependencies
- 生成Gemlock文件，将所用到的所有Gem和其版本都记录下来
- a file located on root directory of your project that defines which source to find gems and dependencies among gems
关于Gemfile：https://ruby-china.org/topics/26655

4. **bundle 和 Gemfile配合生成该目录**的指定Gems及其dependencies
- bundle 会根据 Gemfile 生成 Gemfile.lock，里面锁定了 gem 的版本依赖，在运行的时候会载入指定版本的 gem。
- bundle的作用范围在一个目录里（项目），在某个目录中有Gemfile文件，再运行bundle install，才会根据这个bundle的描述去生成bundle lock，并且bundle lock的作用域只在本目录下,出了本目录，若bundle lock就会报错：“Could not locate Gemfile or .bundle/ directory”。同理，如果没有Gemfile文件在同一个目录，但如果运行bundle install，则会报错“Could not locate Gemfile”

5. **Jekyll**
- Jekyll的核心其实就是一个文本的转换引擎，用你最喜欢的标记语言写文档，可以是Markdown、Textile或者HTML等等，再通过layout将文档拼装起来，根据你设置的URL规则来展现，这些都是通过严格的配置文件来定义，最终的产出就是web页面。    


6. **Homebrew**
- 使用说明： https://segmentfault.com/a/1190000004353419
> Homebrew是OS X上强大的包管理器，为系统软件提供了非常方便的安装方式，独特式的解决了包的依赖问题，并不再需要烦人的sudo，一键式编译，无参数困扰。
> Homebrew将本地的/usr/local初始化为git的工作树，并将目录所有者变更为当前所操作的用户，以后的操作将不需要sudo。
    - bin: 用于存放所安装程序的启动链接（相当于快捷方式）
    - Cellar: 所有brew安装的程序，都将以[程序名/版本号]存放于本目录下
    - etc：brew安装程序的配置文件默认存放路径
    - Library：Homebrew 系统自身文件夹
- homebrew等于是将所有用户安装的包(formula)集中到了同一个文件夹（/usr/local/Cellar）下，接着软链接全部集合在user/local/bin下，让用户安装时不再需要使用sudo：因为使用sudo将以root身份进行操作和安装，一般会影响到所有用户；而mac的文件系统强调的是用户账户分离，每一个用户账户都有独立而相似的文件结构，如果使用sudo进行包的安装，容易在后期发生依赖灾难和位置错误。
- 注意，Homebrew是对OS的包进行管理，比如：整一个ruby是一个包、postgesql也是一个包；但是bundler、RubyGem等ruby里面的包不是macOS的包(对于RubyGem，见下方)
```
brew outdated 
brew
```

7. **Rubygems**  
- RubyGem is a package manager for the Ruby programming language that provides a standard format for distributing Ruby programs and libraries 
- Gem: a packed Ruby program or application. using gem install command means using Rubygems to install gems.
> 包管理工具一般有以下的功能:  
> 1. 注册机制：每个包需要确定唯一的 ID 使得搜索和下载的时候能够正确匹配，所以包管理工具需要维护注册信息，可以依赖其他平台。
> 2. 文件存储：确定文件存放的位置，下载的时候可以找到，当然这个地址在网络上是可访问的。
> 3. 上传下载：这是工具的主要功能，能提高包使用的便利性。比如想用 jquery 只需要 install 一下就可以了，不用到处找下载。上传并不是必备的，根据文件存储的位置而定，但需要有一定的机制保障。
> 4. 依赖分析：这也是包管理工具主要解决的问题之一，既然包之间是有联系的，那么下载的时候就需要处理他们之间的依赖。下载一个包的时候也需要下载依赖的包

8. Ruby, RVM, Gem, bundler, brew & jekyll dependencies (e.g. jekyll-gallery-generator)
它们之间的关系是：
- 先安装rvm，然后选择安装一个ruby版本，就可以提供一个完整的ruby运行环境。之后可以安装brew（brew是用ruby写的）和gem，分别管理os和ruby的软件包。有了gem命令之后再安装bundler，因为bundler本身也是一个gem（ruby应用的统称），直接通过gem命令安装即可。
Refs: http://www.jianshu.com/p/8b13987f392a
http://henter.me/post/ruby-rvm-gem-rake-bundle-rails.html

9. RVM: Mac has builded-in ruby environment but if we want updates then RVM(ruby versions manager) is needed. To install this tool, type following:
```   
curl -L get.rvm.io | bash -s stable
     rvm -v   : check installation completed or not 
     ruby -v : check current ruby version 
     rvm list known: list all ruby version
     rvm install 2.4.4 : install version 2.4.4
```

10. OX11以后的macOX需要更改path因为SIP的设定
- 解决办法http://jekyllrb.com/docs/troubleshooting/#jekyll-amp-mac-os-x-1011
```
通过更改路径：sudo gem install bundler -n /usr/local/bin： 安装bundler
也可以通过brew来统一安装： brew ruby
```




## Reference
- 中文版的gp说明文档：http://wiki.jikexueyuan.com/project/github-pages-basics/jekyll-page.html
- 阮一峰：http://www.ruanyifeng.com/blog/2012/08/blogging_with_jekyll.html
- 另外一篇：http://www.jianshu.com/p/88c9e72978b4
- 快速入门,without any command line: http://azeril.me/blog/Build-Your-First-GitHub-Pages-Blog.html
- 全流程2：https://www.ezlippi.com/blog/2015/03/github-pages-blog.html
- jekyll的liquid语法：http://alfred-sun.github.io/blog/2015/01/10/jekyll-liquid-syntax-documentation/
- 报错示范

```
- Zhengs-MacBook-Pro:heiseningle zhengzhang$ jekyll new .
          Conflict: /Users/zhengzhang/Desktop/Git/heiseningle exists and is not empty.
- Zhengs-MacBook-Pro:heiseningle zhengzhang$ bundle exec jekyll serve
Could not locate Gemfile or .bundle/ directory
-Zhengs-MacBook-Pro:heiseningle.github.io zhengzhang$ jekyll serve
Configuration file: /Users/zhengzhang/Desktop/Git/heiseningle/heiseningle.github.io/_config.yml
       Deprecation: The 'gems' configuration option has been renamed to 'plugins'. Please update your config file accordingly.
  Dependency Error: Yikes! It looks like you don't have jekyll-paginate or one of its dependencies installed. In order to use Jekyll as currently configured, you'll need to install this gem. The full error message from Ruby is: 'cannot load such file -- jekyll-paginate' If you run into trouble, you can find helpful resources at https://jekyllrb.com/help/!
```


## 学习方式
- 在公司学习效率非常低，周六学习效率也非常低，原因是因为总时长长，心中没有目标，没有提前问出来要解决的问题，因此会被各种信息打断，不断地去学习各种旁枝，但最后感觉什么都看了很充实，实际上并没有解决问题；上班是因为 会有各种各样的事情打断，重新回到主线的学习上会非常打断


- 首先要定义问题，所有的研究和学习都是为了解决某个特定的问题，并不是纯粹的信息输入。纯粹的信息输入是一种消极的信息吸收，无法在吸收者的思维中构建信息框架，也就无法长期留存，最后就是看似什么都学了，但是实际都没有留下来。
- 上述这种情况会发生在刷微博、刷知乎等等行为上，纯粹地成为信息吸收者很容易，由此带来的满足感也很迷惑人，但要意识到主动学习才能真正将知识转化为技能，才能将知识变成解决一开始定义的问题的手段。
- 要时刻提醒自己的问题和目标，面对一个新事物—而研究和学习的对象一般都是新的，否则学什么— 必然会遇到各种新名词和疑惑，这个时候google和baidu可以起极大的作用，同时也有可能让人陷入无尽的链接跳转里面，一个接一个不懂的名词，没完没了。因此，需要在阅读过程中做一定的假设，类似于贝叶斯，在先验的基础上去读、去学
