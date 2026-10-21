# fenliu — Quantumult X 分流规则合集

个人自用的 Quantumult X（iOS）分流规则仓库，一个 App 一个 `.list` 文件，全部走代理。

| 文件 | App | 说明 |
|---|---|---|
| `Muse.list` | Muse（Meta） | Muse App 全部流量走代理，含 Meta 登录域名和 AS32934 兜底 |

## 通用用法（以 Muse.list 为例）

在 Quantumult X 配置文件的 `[filter_remote]` 段加一行（策略组名换成你自己的）：

```
https://raw.githubusercontent.com/chirens/fenliu/main/Muse.list, tag=Muse, force-policy=你的代理策略组名, update-interval=86400, opt-parser=false, enabled=true
```

- `force-policy` 填你 QX 里现成的代理策略组名。填了它，`.list` 里每行末尾的 `proxy` 占位会被统一覆盖，不用逐行改。
- 规则要排在 `geoip, cn, direct` 这类国内直连规则**前面**，否则会被抢先匹配走直连。
- 本地粘贴版：把 `.list` 里不以 `#` 开头的行粘到 `[filter_local]` 段，并把每行末尾的 `proxy` 换成你的策略组名。

## Muse.list 说明

- 核心（已核实）：`muse.ai`、`metaaiusercontent.com`（Muse 的内容/分享域名，`cf.metaaiusercontent.com` 在它下面），加一条 `metaai` 关键词兜底。
- 登录/账号：Muse 用 Meta 账号登录，所以带上 `meta.com`、`meta.ai`、`facebook.com`、`facebook.net`、`fb.com`、`fb.me`、`fbcdn.net`、`fbcdn.com`、`fbsbx.com`（`graph.facebook.com`、`accounts.meta.com` 都在这些后缀下面）。
- 兜底：最后一行 `ip-asn, 32934, proxy` 是 Meta 自己的网络（AS32934）。App 万一绕过域名、直连 Meta 的 IP，也会走代理。副作用：Facebook / Instagram / WhatsApp / Meta AI 访问 Meta IP 时也会跟着走代理；不想要就删掉这一行。
- 诚实边界：清单按公开可查的 Muse / Meta 域名整理，`ip-asn` 兜底就是防漏的。用一会儿 Muse 后看 QX 请求日志，若还有走 direct 的 Meta 相关域名，补进清单即可。
