## Mac 装机

### 反标装
标装后无法使用蓝牙、iCloud等系统功能，无法使用QQ等软件。可以在保留访问MAC及MA网络能力的基础上进行反标装。流程如下：
1. 删除其它账户；
2. 删除能删除的描述文件；
3. 使用如下命令删除描述文件；
	- `sudo profiles -P` 查看所有描述文件
	- `sudo profiles -R -p com.apple.mdm.mbp.local.9ba4a5d3-57f1-4c18-ad4a-3b10db0c1fa6.alacarte` 删除描述文件
	>在删除的时候保留MAC网络访问的描述文件，其他的可以删除
4. 删除McAfee `sudo rm -r /usr/local/McAfee`
5. 结束McAfee进程；(可同时Clean My Mac 软件辅助删除)
6. 删除 `jamf`
	- `/usr/local/jamf`
	- `/Library/Application Support/McAfee`
	- 使用Clean My Mac（扩展 --> 启动代理） 清除正在运行的jamf服务。
7. 在 设置 -> 用户与群组 解除域控制
8. 用户与群组 -> 登录选项 ： 将登录窗口显示为：用户列表
9. 重启电脑

### 网络保障
#### 连接MA WiFi网络
- 连接MR WiFi --> 安装证书 --> 输入账号密码 --> 安装profile文件。
- 账号无法使用需要Case重置密码
	- Case地址：http://case.paic.com.cn/case/
	- 操作流程：上报问题—->pc问题目录—->深圳总部ma域账号密码重置申请—->审批人选择牛树民，陈志平
	- 审批完成后会邮件发送地址及密码，修改密码后即可

#### 下载Shaodowsocks 翻墙
- https://github.com/shadowsocks/shadowsocks-iOS/releases/download/2.6.3/ShadowsocksX-2.6.3.dmg

### 软件安装
#### AppStore下载
- 下载[自动登录软件](http://pan.baidu.com/s/1skHg98T)
- 设置--安全性与隐私--隐私--将自动登录软件添加
- 点击自动登录
- 下载app store收费应用

#### Office2016
- 链接：http://officecdn.microsoft.com/pr/C1297A47-86C4-4C1F-97FA-950631F94777/OfficeMac/Microsoft_Office_2016_Installer.pkg
- 激活账号密码：qw487@yeziapp.ml   Office334693

#### Omini
- 下载地址：http://pan.baidu.com/s/1dFe6KPN
- 下载说明在Mac打开，Omni序列号都在里面，看里面的说明安装Omni，请保留此文件，最新的序列号，和常见问题，都会自动在说明内更新。

#### Xcode

#### Cocoapods安装

#### brew 安装

#### Alfred

### 搜狗拼音

#### SublimeText

#### iTerm
- 快捷登录SSH：[Mac下使用iTerm2让SSH免密码登录远程服务器](http://www.tuijiankan.com/2015/05/15/iterm2-mac-ssh-with-no-password/)

#### CleanMyMac

#### SourceTree

#### Chrom
- Postman
- 马克飞象
- 印象笔记剪藏

#### 印象笔记

#### Dropbox
- http://ping.chinaz.com/www.dropbox.com
- http://ping.chinaz.com/dl-web.dropbox.com
- http://ping.chinaz.com/dl.dropboxusercontent.com
- 获得ip后改地址

#### licecap

#### 安装oh-my-zsh
- 地址 https://github.com/robbyrussell/oh-my-zsh
- 命令：sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"

#### 安装xtool，用于自动打包
- `brew install xctool`
