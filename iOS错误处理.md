### 编译错误处理
- 报错：No such file or directory
- 解决方法：
    + 缓存问题：Go to finder -> library -> Developer -> Xcode -> Derived data delete that folder
    + 工程问题（别人更改文件目录后合并代码等导致）:TARGETS --> Build Phases --> search file --> delete redundant Compile Source


### Framework使用错误
#### 动画偶现crash
- 使用`[CABasicAnimation animationWithKeyPath:@"transform.scale"]`会导致报错：`[NSConcreteValue doubleValue]: unrecognized selector sent to instance 0x174135d60`，使用`[CABasicAnimation animationWithKeyPath:@"transform"]`将会没有问题
- [参考文章1](http://stackoverflow.com/questions/7773628/iphone-debugging-a-crash-when-you-cant-find-it/8729649#8729649),[参考文章2](http://stackoverflow.com/questions/9980279/monotouch-nsconcretevalue-doublevalue-unrecognized-selector-sent-to-instanc)
