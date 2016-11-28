## UIKit

### iOS10 Tabbar使用backgroundImage并且隐藏上部的shadow line
```Objective-C
self.tabBar.shadowImage = [UIImage new];
self.tabBar.barStyle = UIBarStyleBlack;
```

### UIWindow
UIWindow可以直接覆盖在当前的屏幕之上，做一些想要的UI效果。使用样例：

```oc
static UIWindow * toastWindow = nil;

+ (void) showWithText:(NSString *)text backgroundColor:(UIColor *)color
{
    if (!toastWindow) {
        UIWindow * window = [UIWindow new];
        window.userInteractionEnabled = NO;
        window.backgroundColor = [UIColor clearColor];
        window.windowLevel = UIWindowLevelStatusBar;
        toastWindow = window;
    }
    
    //不需要使用交互，则不使用makeKeyAndVisible，直接设为不隐藏就可以显示出来。
    toastWindow.hidden = NO;
    
    PAWFFDNToastView *toastView = [[PAWFFDNToastView alloc] initWithView:toastWindow];
    toastView.title = text;
    toastView.backgroundColor = color;
    toastView.alpha = 1.0;
    [toastView showWithAnimated:YES];
    
    //5秒后消失
    dispatch_after(dispatch_time(DISPATCH_TIME_NOW, (int64_t)(5 * NSEC_PER_SEC)), dispatch_get_main_queue(), ^{
        toastWindow.hidden = YES;
    });
}
```

### app资源获取
- iOS 获取当前包中的icon图片，虽然我们项目中配置了很多icon图片，但是在打包后并不能把所有的图片都打进包中，下面的方法可以取得当前包中的icon图片：
```Objective-C
NSString *imageName = [[[[NSBundle mainBundle] infoDictionary] valueForKeyPath:@"CFBundleIcons.CFBundlePrimaryIcon.CFBundleIconFiles"] lastObject];
UIImage * image = [UIImage imageNamed:imageName];
```