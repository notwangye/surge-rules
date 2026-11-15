# Surge 个人直连规则

本目录适用于国内直连白名单、其余流量使用本地代理策略的 Surge 配置。

- `domestic-direct.list`：`.cn`（包括 `.com.cn`）及常用国内服务。从现用白名单起步，对照 [Sukka Domestic Domains](https://ruleset.skk.moe/List/non_ip/domestic.conf) 补充高德、饿了么、爱奇艺、芒果 TV 等常用域名。Sukka 上游许可证见仓库根目录。
- `wechat-ip.list`：五个小范围 IPv4 CIDR。四个前缀与 [blackmatrix7 微信清单](https://github.com/blackmatrix7/ios_rule_script/blob/master/rule/Surge/WeChat/WeChat.list) 对照；另一个覆盖现场观察到的腾讯 MMTLS 地址，参考 [网段归属](https://ipgeolocation.io/browse/ipv4/43.160.156.0/24)。网段可能承载其他业务，遇到异常时可在本地暂停这组规则。未使用整个腾讯 ASN。

在本地 `[Rule]` 的局域网、Apple、券商等指定策略之后，且最后一条 `FINAL` 之前添加：

```ini
RULE-SET,https://raw.githubusercontent.com/notwangye/surge-rules/stable/personal/domestic-direct.list,DIRECT,update-interval=86400
RULE-SET,https://raw.githubusercontent.com/notwangye/surge-rules/stable/personal/wechat-ip.list,DIRECT,no-resolve,update-interval=86400
```

如果本地还有其它同目标规则，Surge 按从上到下的顺序处理。节点定义、凭据及最终代理策略由本地配置管理。稳定版规则从 `stable` 发布；变更先检查差异和范围，再提交发布。此目录为纯规则，不依赖模块、脚本或 HTTPS 解密。
