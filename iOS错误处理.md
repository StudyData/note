### 编译错误处理
- 报错：No such file or directory
- 解决方法：
    + 缓存问题：Go to finder -> library -> Developer -> Xcode -> Derived data delete that folder
    + 工程问题（别人更改文件目录后合并代码等导致）:进入Build Phases，搜索对应的文件，然后删掉无效的Compile Sources内容即可