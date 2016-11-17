### 服务器搭建git仓库
- 1先选定一个目录作为Git仓库，假定是`/srv/sample.git`，在`/srv`目录下输入命令：`sudo git init --bare sample.git`
- 2.Git就会创建一个裸仓库，裸仓库没有工作区，因为服务器上的Git仓库纯粹是为了共享，所以不让用户直接登录到服务器上去改工作区，并且服务器上的Git仓库通常都以.git结尾。然后，把owner改为git：`sudo chown -R git:git sample.git`
- 3.出于安全考虑，第二步创建的git用户不允许登录shell，这可以通过编辑`/etc/passwd`文件完成。找到类似下面的一行：`git:x:1001:1001:,,,:/home/git:/bin/bash`   改为：`git:x:1001:1001:,,,:/home/git:/usr/bin/git-shell`
- 4.克隆远程仓库：`git clone git@server:/srv/sample.git`

### Git日常使用

#### SSH登陆
- 在命令行执行：`$ ssh-keygen -t rsa -C "x@devlxx.com" `会在~目录下创建一个隐藏文件夹`.ssh`，里面有rsa的公钥及私

#### Fork的方式MergeRequest
- 从源仓库`A` fork 出仓库`B`
- 将仓库`B`下载到本地：`git clone B的地址`
- 进入仓库`B`所在目录，与`A`仓库关联：`git remote add upstream A的地址`
- 当`A`仓库有更新需要同步到`B`时，可以在命令行执行：`git fetch upstream`
- 仓库`B`执行合并操作，合并`A`仓库对应的分支，以使代码同步。

#### 镜像 
- `git clone --mirror https://github.com/CocoaPods/Specs.git`    从目标项目镜像
- `git remote set-url --push origin git@git.coding.net:xx-li/Specs.git`   与镜像关联
- `git fetch -p origin`   拉取目标项目最新代码
- `git push --mirror`   推送最新更新到镜像
- 创建脚本定时运行更新：`crontab -e 30 * * * * /mirrors/Specs.git/sync.sh  > /var/log/specssync_`date +"\%Y\%m\%d"`.log 2>&1`
- 碰到的问题：
    + 为http方式时（`git remote set-url --push origin https://git.coding.net/xx-li/Specs.git`）在push时报400错误
    + 为ssh方式时（`git remote set-url --push origin git@git.coding.net:xx-li/Specs.git`）在push时报：`ssh: Could not resolve hostname git.coding.net: Name or service not known。`执行`ping git.coding.net` 得到对应ip `220.243.236.184` ， 然后改为 `git remote set-url --push origin git@220.243.236.184:xx-li/Specs.git` 后OK。（怀疑为国外服务器解析国内域名出现问题）
