## Network

### 允许无效的https证书访问
```Objective-C
//调用的是私有api，可以用于测试环境测试

#if DEBUG

@implementation NSURLRequest (NSURLRequestWithIgnoreSSL) 

+ (BOOL)allowsAnyHTTPSCertificateForHost:(NSString *)host
{
    return YES;
}

@end

#endif
```

### 搭建本地服务器
+ 使用[CocoaHTTPServer](https://github.com/robbiehanson/CocoaHTTPServer)库
+ 样例代码：
```Objective-C
_server = [[HTTPServer alloc] init];
[_server setType:@"_http._tcp."];
[_server setPort:port];
[_server setDocumentRoot:rootPath]; //设定web服务器目录
NSError *error;
if([_server start:&error]) {
    NSLog(@"Started HTTP Server on port %hu", [_server listeningPort]);
}
else {
    NSLog(@"Error starting HTTP Server: %@", error);
}

```

### AFNetworking使用

#### 异常处理
- 网络请求异常Error:
    + `Domain`：`NSURLErrorDomain`
    + code：网络连接问题导致的为：`NSURLErrorNotConnectedToInternet` 与 `NSURLErrorTimedOut`

- 数据解析异常Error：
    + JSON解析问题：


#### Https支持
- 强制验证本地证书：
```Objective-C
AFSecurityPolicy * securityPolicy = [AFSecurityPolicy defaultPolicy];

NSArray * cerNameList = @[@"cer1", @"cer2", @"cer3"];
NSMutableSet * cerSet = [NSMutableSet set];
for (NSString * cerName in cerNameList) {
    NSString *cerPath = [[NSBundle mainBundle] pathForResource:cerName ofType:@"cer"];//证书的路径
    if (!cerPath) {
         NSLog(@"https证书：%@.cer 未找到！", cerName);
    }
    NSData *cerData = [NSData dataWithContentsOfFile:cerPath];
    [cerSet addObject:cerData];
}
 
/*
1. AFSSLPinningModeCertificate 服务器端返回的证书和本地保存的证书中的所有内容，包括PublicKey和证书部分，全部进行校验；如果正确，才继续进行。
2. AFSSLPinningModePublicKey 只验证PublicKey部分
3. AFSSLPinningModeNone  客户端无条件地信任服务器端返回的证书。
*/
securityPolicy = [AFSecurityPolicy policyWithPinningMode:AFSSLPinningModeCertificate withPinnedCertificates:cerSet];

//是否允许无效的证书访问
securityPolicy.allowInvalidCertificates = NO;
//是否验证域名
securityPolicy.validatesDomainName = YES;

_manager = [AFHTTPSessionManager manager];
_manager.securityPolicy = securityPolicy;
```

- 服务器下发证书验证：
```Objective-C
AFSecurityPolicy * securityPolicy = [AFSecurityPolicy defaultPolicy];
securityPolicy.allowInvalidCertificates = NO;
securityPolicy.validatesDomainName = YES;

_manager = [AFHTTPSessionManager manager];
_manager.securityPolicy = securityPolicy;
```
- 域名ATS验证网站：[亚洲诚信](https://www.trustasia.com/tools/ats-checker.htm)

#### 监听网络状态变化
```Objective-C
//监听网络连接状态变化
AFNetworkReachabilityManager * manager = [AFNetworkReachabilityManager sharedManager];
[manager startMonitoring];
__weak __typeof__ (self) wself = self;
[manager setReachabilityStatusChangeBlock:^(AFNetworkReachabilityStatus status) {
    __strong __typeof (wself) sself = wself;
    if (status != AFNetworkReachabilityStatusNotReachable) {
            NSLog(@"网络状况变为可用");
    }
}];
```

#### 同步请求
- 使用NSURLConnection同步请求
```Objective-C
- (NSURLRequest *) urlRequestWithURLString:(NSString *)urlString
                                parameters:(id)parameters
                                     error:(NSError *__autoreleasing *)error
{
    NSDictionary<NSString *, NSString *> *headerFieldValueDictionary =
    @{
      @"xxx":@"xxx",
      };
    
    AFHTTPRequestSerializer *requestSerializer = [AFJSONRequestSerializer serializer];;
    requestSerializer.timeoutInterval = 15;
    for (NSString *httpHeaderField in headerFieldValueDictionary.allKeys) {
        NSString *value = headerFieldValueDictionary[httpHeaderField];
        [requestSerializer setValue:value forHTTPHeaderField:httpHeaderField];
    }
    
    NSURLRequest * request =
    [requestSerializer requestWithMethod:@"POST"
                               URLString:urlString
                              parameters:parameters
                                   error:error];
    return request;
}

NSError * error = nil;
NSURLRequest * request = [self urlRequestWithURLString:urlString
                                                parameters:param
                                                     error:&error];
if (error) {
    return;
}
NSURLResponse * response = nil;
NSData * data = [NSURLConnection sendSynchronousRequest:request returningResponse:&response error:&error];

NSDictionary * responseObject = nil;
if (!error && data) {
    responseObject = [NSJSONSerialization JSONObjectWithData:data options:kNilOptions error:&error];
}
```

- 使用while
```Objective-C
//异步请求完成后将isNeedSync置为NO
while (self.isNeedSync) {
    [[NSRunLoop currentRunLoop] runMode:NSDefaultRunLoopMode beforeDate:[NSDate distantFuture]];
}
```
