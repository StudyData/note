### Python环境搭建
#### 1、安装HomeBrew 
    + 安装命令：`/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"`

#### 2、安装`pip`
+ 1.我们先获取`pip`安装脚本:`wget https://bootstrap.pypa.io/get-pip.py`
如果没有安装`wget`可以去这里将所有内容复制下来,新建`get-pip.py`文件,将内容拷进去就OK了.
+ 2.安装pip: `sudo python get-pip.py`
+ 3.安装pip过慢，可以通过代理来解决：
    * 创建一个文件：~/.pip/pip.conf
    * 修改内容如下：
```
[global]
timeout = 6000
index-url = http://pypi.douban.com/simple/ 
[install]
use-mirrors = true
mirrors = http://pypi.douban.com/simple/ 
trusted-host = pypi.douban.com
```

#### 3、Python虚拟环境`virtualenv`
+ 安装：`$ sudo pip install virtualenv`
+ 创建虚拟环境
```shell
#virtualenv 安装完毕后，你可以立即打开 shell #然后创建你自己的环境。我通常创建一个项目文件夹，并在其下创建一个 venv 文件夹
$ mkdir myproject
$ cd myproject
$ virtualenv venv
New python executable in venv/bin/python
Installing distribute............done.
```

+ 虚拟环境迁移：
    + [虚拟环境迁移](http://yejinxin.github.io/python-pip-virtualenv-usage/)
    + 一般先安装pip，安装好后，pip install virtualenv就可以自动从网上下载并安装virtualenv了。然后virtualenv env1就可以创建一个名为env1的虚拟环境了，进入这个虚拟环境后，再使用pip install安装其它的package就只会安装到这个虚拟环境里，不会影响其它虚拟环境或系统环境。
    + 当需要将虚拟环境env1迁移或复制到另一个虚拟环境（可能不在同一台机器上）env2时，首先仍然需要在目的机器上安装pip和virtualenv，然后采用以下方法之一安装其他的package：
    直接将env1里的文件全部复制到env2里，然后修改涉及路径的文件。此种方法可能正常使用，但显然不是好办法。
    + 进入原虚拟环境env1，然后执行pip freeze > requirements.txt将包依赖信息保存在requirements.txt文件中。然后进入目的虚拟环境env2，执行pip install -r requirements.txt，pip就会自动从网上下载并安装所有包。
