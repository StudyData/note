## sublime使用

### Package Control
#### 安装使用
- Package Control主页：`https://packagecontrol.io`

- 安装方法(Sublime Text 3)：
```python
import urllib.request,os,hashlib; h = 'df21e130d211cfc94d9b0905775a7c0f' + '1e3d39e33b79698005270310898eea76'; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); by = urllib.request.urlopen( 'http://packagecontrol.io/' + pf.replace(' ', '%20')).read(); dh = hashlib.sha256(by).hexdigest(); print('Error validating download (got %s instead of %s), please try manual install' % (dh, h)) if dh != h else open(os.path.join( ipp, pf), 'wb' ).write(by)
```

#### 错误处理
- 弹框报错：There are no packages available for installation
    - `ctrl + ``打开控制台，发现是因为`http://packagecontrol.io/channel_v3.json`获取失败
    - 翻墙，把`channel_v3.json`内容复制下来
    - 将复制下来的json文件上传`七牛`,外链地址为：`http://7viheq.com1.z0.glb.clouddn.com/channel_v3.json`
    - 打开`package control`的的设置文件：`Preference --> Package settings --> Package control --> Settings - User`
    - 加入以下内容并保存
    ```js
    "channels":
    [
        "http://7viheq.com1.z0.glb.clouddn.com/channel_v3.json"
    ],
    ```

### 插件使用
#### Side​Bar​Enhancements
增强侧边栏，文件及目录管理，必装插件。

#### Emmet
HTML/CSS 速写神器。 必装插件。

#### GitHub的Gist使用相关插件
- package controll搜索插件Gist并安装
- 安装gist后，配置github的账户token:
```shell
//执行此命令后，token会打印出来
curl -v -u USERNAME -X POST https://api.github.com/authorizations --data "{\"scopes\":[\"gist\"], \"note\": \"SublimeText 2/3 Gist plugin\"}"
```
- 添加Token。 进入`Preferences --> Package Settings --> Gist --> Setting - User`修改，样例如下：
```js
{
    // Your GitHub API token
    // see: https://github.com/condemil/Gist#generating-access-token
    "token": "a28b686c3a74······c300f42414c7",
}
```

#### Git插件
Git插件排名前25位的有2个，分别是`Git`与`GitGutter`，GitGutter对修改的提示感觉更加友好，选择使用`GitGutter`

#### Anaconda
被人评价为Python终极插件，待验证。