## Swift与脚本

### 构建脚本
- 创建一个hello.swift脚本：
```swift
print("hello swift")
```
- 保存文件后，在命令行输入`$ swift hello.swift`即可执行脚本。

### Shebang与自动执行
- 编写脚本并保存为hello.swift：
```swift
#!/usr/bin/env swift
print("Hello Swift")
```
- 设置执行权限：`$ chomd +x hello.swift`
- 运行脚本：`$ ./hello.swift`

### 在Swift脚本中执行shell脚本
在Swift 脚本里，你可以尽情使用 Swift 语法特性。但有些情况下还是需要调用一些 shell 脚本的。之前版本是用到 Foundation 库中的 NSTask 类。3.0版本后是用Process类样例如下：
```shell
#!/usr/bin/env swift

import Foundation

let shell = "pwd"
let p = Process()
p.launchPath = "/bin/bash"
p.arguments = ["-c", shell]

p.launch()
```


