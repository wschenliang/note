# 蜗牛笔记

> 记录所学所想，努力成就自己

## 笔记内容

+ 专业知识的积累
+ 文献阅读和总结
+ 有趣分章分享
+ 编程学习经验

💡 **「关于」**

- 🎓 仓主目前江南大学研一在读，喜欢研究编程，善于java开发，python开发。
- 🌹 本知识仓库在于平时积累分享，欢迎大家与我交流。
- 🎈 爱好是篮球🏀，乒乓球🎳，电影🎬，动漫喜欢火影海贼，平时也喜欢旅游🚵。
- ❤️ 我的梦想是能做一名大学老师，研究自己喜欢的东西。但知识水平有限，龟速前行。

## 快速部署

### 先安装node.js工具
1. windows安装

    去node.js官网下载安装包
    > https://nodejs.org/en/

2. ubuntu安装

    ```shell
    sudo apt-get install node.js
    ```

3. centos安装
    
    ```shell
    curl -sL https://rpm.nodesource.com/setup_10.x | bash -
    
    yum install -y nodejs
    ```
    [root@localhost /]# node -v
     v10.9.0
     [root@localhost /]# npm -v
     6.2.0

### 全局安装docsify-cli工具
```shell
npm i docsify-cli -g
```
初始化项目，生成三个文件。
- index.html 入口文件
- README.md 会做为主页内容渲染
- .nojekyll  用于阻止 GitHub Pages 忽略掉下划线开头的文件
```shell
docsify init
```
### 启动
```shell
docsify serve
```