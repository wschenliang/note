## 虚拟环境
1.安装虚拟环境
```shell
sudo pip install virtualenv
sudo pip install virtualenvwrapper
```

**安装完虚拟环境后，如果提示找不到mkvirtualenv命令，须配置环境变量：**

```shell
mkdir
$HOME/.virtualenvs
export WORKON_HOME=$HOME/.virtualenvs
source /usr/local/bin/virtualenvwrapper.sh
source ~/.bashrc
```

2.创建虚拟环境
提示：如果不指定python版本，默认安装的是python2的虚拟环境
在python2中，创建虚拟环境：mkvirtualenv 虚拟环境名称
```shell
mkvirtualenv py_flask
```
在python3中，创建虚拟环境：mkvirtualenv -p python3 虚拟环境名称
```shell
mkvirtualenv -p python3 py3_flask
```

**注意：**
- 创建虚拟环境需要联⽹
- 创建成功后, 会⾃动⼯作在这个虚拟环境上
- ⼯作在虚拟环境上, 提示符最前⾯会出现 “虚拟环境名称”

3.使⽤虚拟环境
- 查看虚拟环境命令：workon + 两次tab  或者  lsvirtualenv
- 启动/切换虚拟环境：workon + 虚拟环境名
- 退出虚拟环境：deactivate
- 删除虚拟环境：rmvirtualenv + 环境名
- 先退出，再删除

4.在虚拟环境中安装⼯具包

- ⼯具包安装的位置
  
    - python2版本下
    ```shell
    ~/.virtualenvs/py_flask/lib/python2.7/site-packages/
    ```
    - python3版本下
    ```shell
    ~/.virtualenvs/py3_flask/lib/python3.5/site-packages
    ```

5.安装flask-0.10.1的包
```shell
pip install flask==0.10.1
```
6.查看虚拟环境中安装的包
```shell
pip freeze
```


## 系统性能监控 psutil

```shell
pip3 install psutil
```

```python
import psutil
psutil.users() # 获取⽤户的信息
psutil.boot_time() # 获得开机时间（LINUX格式返回）
datetime.datetime.fromtimestamp(psutil.boot_time()).strftime("%Y-%m-%d %H:%M:%S")
'2017-06-30 17:21:04'
```

## 自动发邮件yagmail

```shell
pip3 install yagmail
```