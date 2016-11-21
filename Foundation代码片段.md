## Foundation

### 清空UserDefault全部数据
- 找到所有的key然后remove掉：
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

- 清除持久域：
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