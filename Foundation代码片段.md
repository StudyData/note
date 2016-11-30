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