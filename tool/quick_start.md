## 安装node.js
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

## 全局安装docsify-cli
```shell
npm i docsify-cli -g
# 初始化项目
docsify init
# 本地启动
docsify serve
```
详情参照官网：
> https://docsify.js.org/#/zh-cn/