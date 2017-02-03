# 规则设置
[Rule] 
# 基于域名判断并屏蔽（REJECT）请求
DOMAIN,pingma.qq.com,REJECT

# 基于域名后缀判断屏蔽（REJECT）请求
DOMAIN-SUFFIX,flurry.com,REJECT

# 基于关键词后缀判断走代理（Proxy），强制不尊重系统代理的请求走 Packet-Tunnel-Provider
DOMAIN-KEYWORD,google,Proxy,force-remote-dns

# 基于域名后缀判断请求走直连（DIRECT）
DOMAIN-SUFFIX,126.net,DIRECT

# Telegram.app 指定“no-resolve”Surge 忽略这个规则与域的请求。 
IP-CIDR,91.108.56.0/22,Proxy,no-resolve 
# 判断是否是局域网，如果是，走直连
IP-CIDR,192.168.0.0/16,DIRECT
# 判断服务器所在地，如果是国内，走直连
GEOIP,CN,DIRECT

# 其他的走代理
FINAL,Proxy