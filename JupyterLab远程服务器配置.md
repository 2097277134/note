## 1. 安装 JupyterLab

```bash
pip install jupyterlab
```

# 2.生成配置文件

```bash
jupyter notebook --generate-config
#生成的文件位于：~/.jupyter/jupyter_notebook_config.py #配置文件
```

# 3. 设置密码

```bash
`$ ipython`
`In [1]: from notebook.auth import passwd`
`In [2]: passwd()`
`Enter password: ******`
`Verify password: ******`
`Out[2]: 'argon2:$xxxxxxxxxxxxxxxxxxxxxxxxxxx'  #这段是密钥`
```

把生成的密钥复制下来后面用，password是远程登录时需要输入的密码。

# 4.修改配置文件

```bash
#vim ~/.jupyter/jupyter_notebook_config.py
c.NotebookApp.ip = '*'
c.NotebookApp.password = u'sha:ce...刚才复制的那个密文'
c.NotebookApp.open_browser = False
c.NotebookApp.port = 8888    #随便指定一个端口，但是要记住
c.NotebookApp.allow_remote_access = True
c.NotebookApp.notebook_dir = u'目录'  #这个是根目录即文件保存目录，不想配置就不配置，默认是用户家目录
```

# 5.安装 Node.js

Windows 和 Linux 如果需要安装拓展插件程序，需要进一步安装 Node.js

```bash
wget https://npm.taobao.org/mirrors/node/v14.5.0/node-v14.5.0-linux-x64.tar.xz    // 下载
tar xf node-v14.5.0-linux-x64.tar.xz                                  // 解压
vim ~/.bashrc
export PATH=/nodejs_file/bin:$PATH //刚才解压Nodejs文件所在的目录
source ~/.bashrc
node -v //检查是否安装成功
```

# 6.JupyterLab 更换 Kernel

有的时候我们需要不同的环境来执行不同的项目，这个时候 Kernel 就起了作用，切换不同 Kernel 我们就拥有了不同的环境。当你想创建新的 kernel 并且想要在远程服务器上运行该 kernel 时。跟着下面做就对了：

## 1.conda 指令创建新环境

```bash
conda create --name [yourEnvName] python=[VERSION_NUM]
```

远程服务器切换环境到新的 kernel, 比如我这边创建了一个叫 test 的环境

```bash
conda activate test
```

## 2.安装 ipykernel

```bash
(test)$ conda install nb_conda_kernels
```



## 3.将环境写入 JupyterLab 的 Kernel 中

```bash
(test)$ python -m ipykernel install --user --name 环境名称 --display-name "显示的名称"
```

# 7.启动 JupyterLab

```bash
(base)$ jupyter-lab
```

# 8.远程访问

在本地浏览器里输入下面网址, 然后输入最开始设置的密码就能够使用 JupyterLab 了

