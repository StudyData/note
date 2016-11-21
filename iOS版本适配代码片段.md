## 版本适配

### ATS适配
- 添加白名单：
```XML
<key>NSAppTransportSecurity</key>
    <dict>
        <key>NSExceptionDomains</key>
        <dict>
            <key>baidu.com</key>
            <dict>
                <key>NSExceptionAllowsInsecureHTTPLoads</key>
                <true/>
                <key>NSIncludesSubdomains</key>
                <true/>
                <key>NSExceptionRequiresForwardSecrecy</key>
                <false/>
            </dict>
        </dict>
    </dict>
</dict>

```

- 直接禁用ATS
`NSAllowsArbitraryLoads` 设为`YES`



