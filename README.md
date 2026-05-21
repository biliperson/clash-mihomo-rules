# clash-mihomo-rules

原始链接: 

https://raw.githubusercontent.com/biliperson/clash-mihomo-rules/refs/heads/main/clash/gemini.yaml

针对 raw.githubusercontent.com 在国内经常遭到 DNS 污染或 SNI 阻断导致无法拉取规则的问题，以下是几个常用的国内加速/反向代理（镜像）转换方案，并附带转换后的实际链接及解析。

1. jsDelivr (以 testingcf/fastly 节点为例)
转换后的链接:

https://testingcf.jsdelivr.net/gh/biliperson/clash-mihomo-rules@main/clash/gemini.yaml

(也可以将 testingcf 替换为 cdn 或 fastly)

转换的规则:
提取原链接中的关键元素，按 jsDelivr 的格式重组：
https://testingcf.jsdelivr.net/gh/
{用户名}/{仓库名}@{分支名}/{文件相对路径}
(注意：原链接中的 refs/heads/main 在 jsDelivr 中缩写为 @main)

谁、如何转换的:
jsDelivr 是一家提供公共免费 CDN 服务的平台。当用户请求上述链接时，jsDelivr 的边缘服务器节点（依托 Cloudflare 或 Fastly 等大型网络）会代为向 GitHub 发起请求，将抓取到的 gemini.yaml 文件缓存在自己的全球 CDN 节点上。国内用户下载时，流量走的是 jsDelivr 的 CDN 专线，从而绕过针对 GitHub 直接访问的阻断。

2. GHProxy (通用 GitHub 代理)
转换后的链接:

https://ghproxy.net/https://raw.githubusercontent.com/biliperson/clash-mihomo-rules/refs/heads/main/clash/gemini.yaml

(备用域名：[https://mirror.ghproxy.com/]

转换的规则:
直接在原完整链接前方拼接代理网址：
https://ghproxy.net/
{原始完整 GitHub URL}

谁、如何转换的:
开源社区开发者（如基于 hunshcn/gh-proxy 项目搭建的各路公益节点）。这些服务通常部署在 Cloudflare Workers 或海外自建 VPS（Nginx 反向代理）上。它充当了一个“中间人”——国内用户向 GHProxy 的服务器发送请求，GHProxy 服务器在海外不受限的网络环境中下载原 GitHub 文件，再以数据流的形式原样返回给国内用户。

3. GitMirror (直接域名替换)
转换后的链接:

https://raw.gitmirror.com/biliperson/clash-mihomo-rules/refs/heads/main/clash/gemini.yaml

转换的规则:
保持原链接的所有路径结构不变，仅替换域名：
将 raw.githubusercontent.com 直接替换为 raw.gitmirror.com。

谁、如何转换的:
GitMirror 公益社区。他们在海外服务器配置了针对 GitHub 各大域名的强制路由重写。底层的实现依然是反向代理，但通过保持与官方完全一致的 URL 路径结构，极大地方便了脚本和客户端的批量替换（只需做一个全局字符串替换操作），用户端通过 GitMirror 尚未被封锁的域名和 IP 与海外服务器建立连接，获取规则文件。
