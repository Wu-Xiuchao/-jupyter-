# -jupyter-

*今天准备在云端搭建jupyter 实现远程访问，浏览了网上的一些资料，但是大多数都有坑，所以在这里总结一些我实现的方法*
*腾讯云服务器，centos系统,因为其python 版本为2.7 而IPython 现在的版本需要3.5以上,所以需要先更新python 版本*
*参考博客 [https://blog.csdn.net/qq_23845067/article/details/80474003 ]*

## 安装依赖环境
``yum -y install zlib-devel bzip2-devel openssl-devel ncurses-devel sqlite-devel readline-devel tk-devel gdbm-devel db4-devel libpcap-devel xz-devel``

## python3.x下载
``wget https://www.python.org/ftp/python/3.6.1/Python-3.6.1.tgz``

## 解压python 3.x
``tar -zxvf Python-3.6.1.tgz``

## 安装python 3.x
``cd Python-3.6.1 ``
``./configure –prefix=/usr/local/python3``
``make && make install``

## 为python3.x创建一个文件并建立软链接
``mkdir -p /usr/local/python3``
``ln -s /usr/local/python3/bin/python3 /usr/bin/python3``

## 修改环境变量
``vim ~/.bash_profile``
打开文件后，添加变量/usr/local/python3/bin，用:隔开

## 立即启用变量
``source ~/.bash_profile``

## 安装jupyter 
``pip3 install -U jupyter``

## 获取hash密码(用于远程登录jupyter所用，就是把你的密码映射成字符串)
``In [1]: from notebook.auth import passwd``
``In [2]: passwd()``
``Enter password:``
``Verify password:``
``Out[2]: 'sha1:xxxxxxxxxxxxxxxxx'``

## 现在需要jupyter 的配置文件，生成一下
``jupyter notebook –generate-config ``
``vim ~/.jupyter/jupyter_notebook_config.py``

## 修改文件
``c.NotebookApp.allow_origin = '*' #允许其他接入``
``c.NotebookApp.ip = '0.0.0.0' # 监听所有IP``
``c.NotebookApp.open_browser = False # 不打开浏览器``
``c.NotebookApp.port = 7000 #端口``
``c.NotebookApp.notebook_dir = u’jupyter’ #这个是你远程接入jupyter的主目录,运行jupyter必须是这个目录的父目录``
``c.NotebookApp.password = 'sha1:xxxxxxxxxxxxxxx' #前面的密码``

## 可以运行了
``nohup jupyter notebook --allow-root &  #后台运行即可``

## 截图
![jupyter](https://github.com/Wu-Xiuchao/-jupyter-/blob/master/1.png)



