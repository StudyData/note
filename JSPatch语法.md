## JSPatch语法
接入JSPatch，需要将OC原生代码转换为JS代码

### 相关资料
- [JSPatch基础用法](https://github.com/bang590/JSPatch/wiki/JSPatch-%E5%9F%BA%E7%A1%80%E7%94%A8%E6%B3%95#%E8%A6%86%E7%9B%96%E6%96%B9%E6%B3%95)

### 实际使用碰到的坑
- CGRectMake(100, 120, 175, 100) 使用 {x:100, y:120, width:175, height:100} 代替。
- 覆盖类方法
```js
defineClass('LXDemoVC', {
    viewWillAppear: function(animated) {
        //do someting
    },
});
```
- 调用未覆盖前的 OC 原方法:
```js
defineClass('LXDemoVC', {
    viewWillAppear: function(animated) {
        self.ORIGviewWillAppear(animated); // 调用未覆盖前的 OC 原方法
        //do someting
    },
});
```
