---
title: hexo next 搜索 algolia
categories: 工具
tags: [hexo]
toc: true
date: 2017-10-30 22:48:47
scaffolds:
---
# 1. 目的:添加搜索功能
algolia似乎是试用一段时间就不能用了,建议使用本地搜索  
[hexo-next 本地搜索](/2017/11/14/hexo-next-%E6%9C%AC%E5%9C%B0%E6%90%9C%E7%B4%A2/#more)  

# 2. next版本
我的版本号是
```java
    # Theme version
    version: 5.1.2
```
next5.0+版本集成了algolia 这样使配置algolia更简单

# 3. algolia账号申请
[Algolia官网](https://www.algolia.com/)
<!-- more -->
* 直接用github账号注册就好啦
* 创建index空间 输入一个名称,填写你自己站点的网址

![1](http://blogimage.signalfire2017.com/hexo/QQ%E6%88%AA%E5%9B%BE20171030220402.jpg)
* 创建两个key   
    1. 空间创建好后默认会有一个只有搜索权限的key  
    2. 需要另建一个有修改记录等权限的key(这个供我们提交索引到空间使用)  
    3. 创建key的时候要选择授权的空间

![3](http://blogimage.signalfire2017.com/hexo/QQ%E6%88%AA%E5%9B%BE20171030220635.jpg)
![2](http://blogimage.signalfire2017.com/hexo/QQ%E6%88%AA%E5%9B%BE20171030220735.jpg)
![4](http://blogimage.signalfire2017.com/hexo/QQ%E6%88%AA%E5%9B%BE20171030220846.jpg)
![5](http://blogimage.signalfire2017.com/hexo/QQ%E6%88%AA%E5%9B%BE20171030220920.jpg)
* 记录  
    1. Application ID  
    2. 两个key建

# 4. 安装hexo-algolia

用git-bash在hexo工程根目录下执行

    npm install hexo-algolia --save
# 5. 配置algolia
- 在Hexo工程根目录的_config.yml中加入如下配置，注意改成前面API Keys页面相应配置

```java
    algolia:
        applicationID: '你的Application ID'
        apiKey: '只有搜索权限的key'
        adminApiKey: ''
        indexName: '你的index空间名称'
        chunkSize: 5000
```

- 修改themes>next>_config.yml 

    搜索 algolia_search 修改enable 为true  
    其他字体提示可以自己随意修改
![2](http://blogimage.signalfire2017.com/hexo/QQ%E6%88%AA%E5%9B%BE20171030223530.jpg)
# 6. 添加环境变量
我的电脑>右键属性>高级设置>环境变量>新建>填写变量名称和变量值

    变量名称 : HEXO_ALGOLIA_INDEXING_KEY
    变量值: 在algolia新建的有修改权限的key
![1](http://blogimage.signalfire2017.com/hexo/QQ%E6%88%AA%E5%9B%BE20171030222934.jpg)
![2](http://blogimage.signalfire2017.com/hexo/QQ%E6%88%AA%E5%9B%BE20171030222951.jpg)
# 7. 生成index上传到algolia
在hexo根目录执行,**注意确保命令行面板已经重新载入新加的环境变量**
```java
    hexo algolia
```

不报错就可以啦

# 8. 参考
[NexT主题集成Algolia搜索插件](http://blog.csdn.net/luzheqi/article/details/52798557)  
[Hexo集成Algolia搜索插件](https://jobbym.github.io/2017/01/16/Hexo%E9%9B%86%E6%88%90Algolia%E6%90%9C%E7%B4%A2%E6%8F%92%E4%BB%B6/)
