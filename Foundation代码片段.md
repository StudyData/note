## Foundation

### 清空UserDefault全部数据
- 方法一：找到所有的key然后remove掉：
```Objective-C
/** 
 *  清除所有的存储本地的数据 
 */  
- (void)clearAllUserDefaultsData  
{  
    NSUserDefaults *userDefaults = [NSUserDefaults standardUserDefaults];  
      
    NSDictionary *dic = [userDefaults dictionaryRepresentation];  
    for (id  key in dic) {  
        [userDefaults removeObjectForKey:key];  
    }  
    [userDefaults synchronize];  
}  
```

- 方法二：清除持久域：
```Objective-C
/** 
 *  清除所有的存储本地的数据 
 */  
- (void)clearAllUserDefaultsData  
{  
    NSString *appDomain = [[NSBundle mainBundle] bundleIdentifier];  
    [[NSUserDefaults standardUserDefaults] removePersistentDomainForName:appDomain];  
}  
```

### 存储文件到硬盘
#### 注意点
当直接使用`NSHomeDirectory()`后面拼接路径去存储文件到本地时，是会失败的，应该是没有对主目录操作的权限，但是对它的下级目录，如Documents等则可以在里面创建文件及目录
```Objective-C
//直接在home目录下操作
NSString * testPath = NSHomeDirectory(); 
NSString *filePath1 = [testPath stringByAppendingPathComponent: @"test.archive"];

//在home目录下的子目录Documents下操作
NSArray *paths = NSSearchPathForDirectoriesInDomains(NSDocumentDirectory, NSUserDomainMask, YES);
NSString *documentsDirectory = [paths objectAtIndex:0];
NSString *filePath2 = [documentsDirectory stringByAppendingPathComponent: @"test.archive"];

[NSKeyedArchiver archiveRootObject:@"test" toFile:filePath1];   //失败
[NSKeyedArchiver archiveRootObject:@"test" toFile:filePath2];   //成功

```

### crash收集
- [iOS崩溃调试的使用和技巧总结](http://www.cocoachina.com/ios/20151218/14748.html)
- 系统自带的crash收集
```Objective-C
/*
注意：第三方统计工具并不是用的越多越好，使用多个崩溃收集第三方会导致NSSetUncaughtExceptionHandler()函数指针的恶意覆盖，导致有些第三方不能收到崩溃信息。
现在很多第三方崩溃收集工具为了确保自己能最大可能的收集到崩溃信息，会对NSSetUncaughtExceptionHandler()函数指针的恶意覆盖。因为这个函数是将函数地址当做参数传递，所以只要重复调用就会被覆盖，这样就不能保证崩溃收集的稳定性。

注：问过听云的，说只要在main()函数中嵌码的话那么二者都能采集。原理不明。
*/
//需要捕获的signal
static int s_fatal_signals[] = {
    SIGABRT,
    SIGBUS,
    SIGFPE,
    SIGILL,
    SIGSEGV,
    SIGTRAP,
    SIGTERM,
    SIGKILL
};

static int s_fatal_signal_num = sizeof(s_fatal_signals)/sizeof(s_fatal_signals[0]);

void UncaughtExceptionHandler(NSException *exception) {
    NSArray *arr = [exception callStackSymbols];//得到当前调用栈信息
    NSString *reason = [exception reason];//非常重要，就是崩溃的原因
    NSString *name = [exception name];//异常类型
}

void SignalHandler(int code) {
    NSLog(@"signal handler = %d",code);
}

void InitCrashReport() {
    // 1 linux错误信号捕获
    for (int i = 0; i < s_fatal_signal_num; ++i) {
        signal(s_fatal_signals[i], SignalHandler);
    }
    // 2 objective-c未捕获异常的捕获
    NSSetUncaughtExceptionHandler(&UncaughtExceptionHandler);
}

int main(int argc, char * argv[]) {
    @autoreleasepool {
        InitCrashReport();
        return UIApplicationMain(argc, argv, nil, NSStringFromClass([AppDelegate class]));
    }
}
```

- 第三方开源框架[PLCrashReporter](https://www.plcrashreporter.org/)
