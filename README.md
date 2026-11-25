# proxy-pool

自动聚合多个公开免费节点源，去重后生成 **Clash** 和 **v2rayN** 两种订阅，每 6 小时由 GitHub Actions 自动更新。

## 订阅地址

| 客户端 | 订阅链接（墙内优先用镜像） |
|---|---|
| **Clash / Mihomo / FlClash / Clash Verge** | `https://gh-proxy.com/raw.githubusercontent.com/yongce-liu/proxy-pool/main/clash.yaml` |
| **v2rayN / v2rayNG / Shadowrocket** | `https://gh-proxy.com/raw.githubusercontent.com/yongce-liu/proxy-pool/main/v2ray.txt` |

**日常订阅优先使用上面的镜像链接**，尤其是 GitHub 原始地址访问不稳定时。Clash 用户只需要复制 Clash 那一行。

原始地址（海外或已有代理时可用）：

```
https://raw.githubusercontent.com/yongce-liu/proxy-pool/main/clash.yaml
https://raw.githubusercontent.com/yongce-liu/proxy-pool/main/v2ray.txt
```

仓库地址：<https://github.com/yongce-liu/proxy-pool>。GitHub Actions 每 6 小时自动更新订阅文件。

## Clash 版内置策略组

- **🚀 节点选择**：总开关，只显示少量入口，不再把所有节点平铺在这里
- **⚡ 精选最快**：来自经过额外测活/校验的来源，优先用于日常使用
- **⚡ 自动最快**：包含全部节点的 url-test 组，每 5 分钟以 `youtube.com/generate_204` 测速并自动切换最快节点
- **🌍 国家/地区**：二级下拉菜单，按国家/地区列出节点组；每个国家组都是 url-test 组，只在该国节点内自动选最快
- **🧩 全部节点**：需要手动指定单个节点时再进入这个三级菜单
- 国别识别：优先解析节点名（国旗 emoji / 中文国名 / ISO 代码），识别失败的再用 GeoIP 按服务器 IP 归属地补识别

策略组层级如下：

```
🚀 节点选择
├── ⚡ 精选最快
├── ⚡ 自动最快
├── 🌍 国家/地区
│   ├── 🇭🇰 香港
│   ├── 🇯🇵 日本
│   ├── 🇺🇸 美国
│   └── ...
├── 🧩 全部节点
└── DIRECT
```

国家组名称会带节点数量（例如 `🇯🇵 日本·40`），方便判断该组规模；无法识别归属的节点统一放入 `🏳️ 未标注`。

分流规则：内网 IP 直连 → GEOIP CN 直连 → 其余走代理。

## 使用建议

1. 客户端把订阅**自动更新间隔设为 6 小时**（与仓库更新节奏一致）
2. 日常优先选 **⚡ 精选最快**，它的来源做过测活或格式校验，节点规模也更可控
3. 想从全部池子中选最快节点时选 **⚡ 自动最快**；首次测速会比精选组慢
4. 想固定国家时选 **🌍 国家/地区**，再选具体国家；国家组会自动使用该国延迟最低的节点
5. 免费节点波动大，需要指定节点时进入 **🧩 全部节点**

## 本地运行

项目使用 [uv](https://docs.astral.sh/uv/) 管理 Python 依赖：

```bash
uv sync --frozen
uv run --frozen python scripts/aggregate.py
```

## 上游源

[freeSub](https://github.com/Ruk1ng001/freeSub) · [NoMoreWalls](https://github.com/peasoft/NoMoreWalls) · [AutoMergePublicNodes](https://github.com/chengaopan/AutoMergePublicNodes) · [zhuhaiuk/free-nodes](https://github.com/zhuhaiuk/free-nodes) · [free18](https://github.com/free18/v2ray) · [get_subscribe](https://github.com/ermaozi/get_subscribe) · [clashfree](https://github.com/free-nodes/clashfree) · [V2RayAggregator](https://github.com/mahdibland/V2RayAggregator) · [Free-servers](https://github.com/Pawdroid/Free-servers) · [0xRadikal](https://github.com/0xRadikal/Free-v2ray-Configs) · [morpheusadam](https://github.com/morpheusadam/v2ray-config) · [nodesradar](https://github.com/nodesradar/nodesradar-en) · [Free-Nodes](https://github.com/735754647/Free-Nodes) · [free-proxy-airport](https://github.com/sunmiao4458/free-proxy-airport) · [free_proxies](https://github.com/vxiaov/free_proxies) · [awesome-vpn](https://github.com/awesome-vpn/awesome-vpn)

> 默认源优先选择近期活跃、带测活或格式校验的来源；Epodonios、MhdiTaheri、Alirewa、miladtahanian 等 Telegram 收集器因服务器大面积被墙未默认收录

### 可选的大池子（默认未接入）

- [Barabama/FreeNodes](https://github.com/Barabama/FreeNodes)：节点量很大，但包含较多 HTTP/SOCKS 原始节点，不能保证每个都经过真实代理测速。
- [anaer/Sub](https://github.com/anaer/Sub)：能带来较多增量节点，但原始池子波动大，需要二次测活。
- [Alirewa/V2ray-Configs](https://github.com/Alirewa/V2ray-Configs)：Telegram 收集器，数量多、重复和失效节点也更多。

这些源适合继续做“候选池”，不建议直接塞进顶层自动组。当前默认只接入带测活或格式校验的源，避免节点数量变多但实际可用率下降。

## 当前统计

<!--STATS-->
更新于 **2026-11-25 09:58:44 北京时间**，共 **3038** 个去重节点

| 上游源 | 节点数 |
|---|---|
| freeSub | 107 |
| NoMoreWalls | 275 |
| AutoMerge | 42 |
| zhuhaiuk | 27 |
| free18 | 100 |
| ermaozi | 44 |
| clashfree | 1 |
| Eternity | 133 |
| Pawdroid | 2 |
| 0xRadikal | 646 |
| morpheus | 1547 |
| nodesradar | 66 |
| 735754647 | 11 |
| sunmiao4458 | 7 |
| vxiaov | 28 |
| awesome-vpn | 2 |
<!--/STATS-->

## 声明

节点均来自互联网公开资源，仅供学习交流，请遵守当地法律法规。
