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