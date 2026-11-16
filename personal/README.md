# Surge 个人直连规则

本目录适用于国内直连白名单、其余流量使用本地代理策略的 Surge 配置。完整说明与变更记录见[仓库首页](https://github.com/notwangye/surge-rules)。

- `domestic-v4.list`：`.cn`（包括 `.com.cn`）及常用国内服务。从现用白名单起步，对照 [Sukka Domestic Domains](https://ruleset.skk.moe/List/non_ip/domestic.conf) 补充高德、饿了么、爱奇艺、芒果 TV 等常用域名。另含 6 个已核对归属的阿里系 IPv4 网段。Sukka 上游许可证见仓库根目录。
- `apple-direct.list`：Apple 与 iCloud 域名，使用直连。
- `ibkr.list`：盈透证券域名，交给本地 IBKR 策略组。
- `wechat-ip.list`：六个小范围 IPv4 CIDR。四个前缀与 [blackmatrix7 微信清单](https://github.com/blackmatrix7/ios_rule_script/blob/master/rule/Surge/WeChat/WeChat.list) 对照；另外两个覆盖现场观察到的腾讯 MMTLS 地址，参考 [43.160.156.0/24 网段归属](https://ipgeolocation.io/browse/ipv4/43.160.156.0/24) 和 [43.144.0.0/16 上级网段归属](https://ipgeolocation.io/browse/ipv4/43.144.0.0/16)。这些网段也可能承载其他腾讯业务。

在本地 `[Rule]` 中保留节点域名直连和 `LAN`，依次引用以下规则，最后保留本地 `FINAL`：

```ini
RULE-SET,https://raw.githubusercontent.com/notwangye/surge-rules/stable/personal/ibkr.list,IBKR,update-interval=86400
RULE-SET,https://raw.githubusercontent.com/notwangye/surge-rules/stable/personal/apple-direct.list,DIRECT,update-interval=86400
RULE-SET,https://raw.githubusercontent.com/notwangye/surge-rules/stable/personal/domestic-v4.list,DIRECT,update-interval=86400
RULE-SET,https://raw.githubusercontent.com/notwangye/surge-rules/stable/personal/wechat-ip.list,DIRECT,no-resolve,update-interval=86400
```

设备凭据和出站策略留在本机。Surge 每 24 小时重新下载规则。更新上游内容前先审查差异；规则文件只包含匹配条件。
