# Xcode使用

## CodeSnippets
可以使用Github同步Xcode的代码片段，以便多设备共用同步。

- Xcode代码片段所在目录：`~/Library/Developer/Xcode/UserData/CodeSnippets`

## Xcode8支持ios8以下真机测试方法
- 1. 应用程序-xcode 显示包内容-Contents-Developer-Platforms-iPhoneOS.platform-DeviceSupport 把里边 6.0 6.1 7.0 7.1 的文件夹粘贴到xcode8 对应的文件夹内  
- 2. 应用程序-xcode 显示包内容-Contents-Developer-Platforms-iPhoneOS.platform-Developer-SDKs-iPhoneOS.sdk-SDKSettings.plist 文件下DefaultProperties - DEPLOYMENT_TARGET_SUGGESTE... 该数组中添加 6.0 6.1 7.0 7.1 对应的测试版本,

>注意:1. 如果你的文件是只读模式的,那么是不能修改的,你需要把Contents-Developer-Platforms-iPhoneOS.platform-Developer-SDKs-iPhoneOS.sdk-SDKSettings.plist 这些文件的只读模式都改成读写模式，单独改文件的权限不行，改了上级目录的权限就OK了
2. 这个版本排序一定要是从小到大,直接把小的添加到下面是不管用的,必须把小的拖到最上边.这个时候退出你的Xcode,然后重新启动,你就会发现ios8.0以下的真机 也可以正常测试了

## 定位到指定行
`command + L`，输入行号，回车即可定位到指定行