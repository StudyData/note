## Network

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

