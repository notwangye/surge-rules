# Surge 分流规则

供 Surge iOS 和 macOS 共用。设备连接新加坡或美国节点，分流清单从 [`stable/personal/`](https://github.com/notwangye/surge-rules/tree/stable/personal) 读取。

## 当前策略

| 流量 | 策略 | 规则文件 |
| --- | --- | --- |
| 局域网及代理节点域名 | 直连，由本机配置处理 | |
| IBKR | 手动选择，默认直连 | [ibkr.list](https://github.com/notwangye/surge-rules/blob/stable/personal/ibkr.list) |
| Apple、iCloud | 直连 | [apple-direct.list](https://github.com/notwangye/surge-rules/blob/stable/personal/apple-direct.list) |
| `.cn`（含 `.com.cn`）、常见国内服务、已观察到的阿里系 IP 网段 | 直连 | [domestic-v4.list](https://github.com/notwangye/surge-rules/blob/stable/personal/domestic-v4.list) |
| 已观察到的微信 IP 网段 | 直连 | [wechat-ip.list](https://github.com/notwangye/surge-rules/blob/stable/personal/wechat-ip.list) |
| 其余流量 | 交给本机 `Outbound`。默认新加坡优先、美国备用，也可手动选择出口 | |

## Surge 引用方式

在本地 `[Rule]` 中，把节点域名和 `LAN` 的直连规则放前面，接上这四条：

```ini
RULE-SET,https://raw.githubusercontent.com/notwangye/surge-rules/stable/personal/ibkr.list,IBKR,update-interval=86400
RULE-SET,https://raw.githubusercontent.com/notwangye/surge-rules/stable/personal/apple-direct.list,DIRECT,update-interval=86400
RULE-SET,https://raw.githubusercontent.com/notwangye/surge-rules/stable/personal/domestic-v4.list,DIRECT,update-interval=86400
RULE-SET,https://raw.githubusercontent.com/notwangye/surge-rules/stable/personal/wechat-ip.list,DIRECT,no-resolve,update-interval=86400
FINAL,Outbound
```

`IBKR` 和 `Outbound` 由本机 `[Proxy Group]` 定义。各设备的节点密码保存在各自的本地配置里。更新 `stable` 上的规则清单后，Surge 会按 24 小时的刷新间隔重新下载，也可以手动刷新。

## 变更记录

| 版本 | 变更 |
| --- | --- |
| v1 | 首版采用指定国际服务走代理，其余流量直连。 |
| v2 | 改为国内直连白名单，其余默认走新加坡，减少未单列的国际服务误走直连。 |
| v3 | 开始由 GitHub 发布国内域名和微信 IP 清单。 |
| v4 | 国内、Apple、微信 IP 和 IBKR 四份清单统一迁到 GitHub。本地保留节点、局域网、策略组和兜底规则。 |
| v4 规则更新 | 微信增加 `43.144.113.0/24`，现有 6 个 IPv4 直连网段。国内清单增加截图中核对过归属的 6 个阿里系 `/24` 网段。 |
| v5 | 增加美国节点和 `AutoFailover`，默认新加坡优先、美国备用。iPhone 和 Mac 使用独立节点凭据。 |

## 来源与许可

本仓库 fork 自 [SukkaW/Surge](https://github.com/SukkaW/Surge)。国内域名参考 [Sukka Domestic Domains](https://ruleset.skk.moe/List/non_ip/domestic.conf)，微信网段参考 [blackmatrix7 微信清单](https://github.com/blackmatrix7/ios_rule_script/blob/master/rule/Surge/WeChat/WeChat.list) 和实际请求记录。上游适用 [AGPL-3.0](LICENSE)，其中国大陆 IPv4 路由文件按上游说明采用 CC BY-SA 2.0。
