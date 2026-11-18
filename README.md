# digitalocean替代怎么选：Vultr、Hetzner、Linode 谁更便宜？换过去要重新学什么？备份和流量怎么算进真实成本（附四条路线对比）

DigitalOcean 入门 Droplet 目前 **$4.00/月**：512 MiB 内存、10 GiB 存储、500 GiB 月流量、1 vCPU，小时价 **$0.00595**，按秒计费，最低收 60 秒或 $0.01（取较高者）。同一个入门档，Vultr 的 Regular Performance 是 **$2.50/月**，0.5 GB 内存、10 GB SSD、0.50 TB 流量，小时价 $0.004；Hetzner 的 CX23 是 **$4.09/月**，给 2 vCPU、4 GB 内存、40 GB NVMe 和 20 TB 月流量。三组数字摆在一起，换不换、换去哪，基本就有方向了。

先看你卡在哪一项。嫌入门月费贵，Vultr 是同一种自助管理模式下最省事的换法，配置几乎一一对应；觉得 512 MiB 不够用、又不想一次跳太远，Linode 的 $5/月给 1 GB 内存、25 GB 存储和 1 TB 流量；业务主要在欧洲、出口流量大，Hetzner 的 20 TB 额度能消掉一整块不可预测的账单；真正让你难受的是“不想再自己打补丁、盯监控”，那看的是 Cloudways 这类托管层，算力单价反而次要。LightLayer 是另一条按区域和线路挑轻量云的路子，官方新闻页挂出的圣何塞 SJ-Lite Cloud 促销价 **$2.45/月**，原价 $4.9/月。

下面按“你卡在哪一项”来拆，不按厂商目录排。先给选购速览，再把 LightLayer 官网能确认的套餐列清楚，然后逐条讲 DigitalOcean 的真实短板、四条替代路线的已核验价格、备份和流量怎么改变总成本，最后给谁不该换和迁移清单。

## 选购速览：多数人属于哪一类

> - 想保留同样的自助模式、把月费压下来：Vultr，$2.50/月，0.5 GB 内存、10 GB SSD、0.50 TB 流量，$0.004/hr。
> - 入门配置不够用，还想留一条按实例买托管的退路：Linode，$5/月给 1 GB 内存、25 GB 存储、1 TB 流量，托管加购 $100/实例/月。
> - 欧洲业务、出口流量大：Hetzner CX23，$4.09/月，2 vCPU、4 GB 内存、40 GB NVMe、20 TB 流量。
> - 不想自己运维：Cloudways，$14/月起，买的是托管层，算力本身不是重点。
> - 想按区域和线路挑轻量云、预算压在 $3 上下：LightLayer 的圣何塞 SJ-Lite Cloud，促销 $2.45/月。

👉 [查看 LightLayer 当前套餐与价格](https://account.lightlayer.net/?affid=74)

这些是各家公开入门价。Hetzner 同时有美元和欧元两个口径，CX23 的 $4.09/月是美元价，另有 €3.79/月起和 CAX €3.99/月起的欧元口径，对应不同产品线，不能混成一条来比。

## LightLayer 的套餐与价格：官网能确认的部分

LightLayer 的账户中心有套餐选购入口，公开页面没有一张把全部区域、全部配置列全的价目表。官方在 2026-09-24 的新闻里公布了 SJ-Lite Cloud 的配置和促销价，这是目前能从官方渠道直接核验的一条。

| 套餐/产品 | 核心配置 | 价格 | 计费周期 | 购买入口 |
| --- | --- | --- | --- | --- |
| SJ-Lite Cloud（圣何塞） | 1 核 / 1 GB / 50 GB HDD / 1 IP / 300M–500G 优化带宽 | 原价 $4.9/月，5 折促销 $2.45/月 | 月付 | [查看当前价格](https://account.lightlayer.net/?affid=74) |
| 其他云服务器与独立服务器 | 区域、配置、线路在选购页选择 | 公开页面未完整列价，以选购页为准 | 月付 / 年付 | [进入选购页](https://account.lightlayer.net/?affid=74) |

SJ-Lite Cloud 的官方定位是“圣何塞经济型云服务器，适合轻量应用及基础使用场景”。硬盘是 HDD，这一点下单前要看清：它跑得动 WordPress、开发环境和轻量应用，数据库、高并发读写这类负载不要指望 HDD 的随机 IOPS。IP 给 1 个，带宽标注 300M–500G 优化带宽，计费周期是月付。

促销价 $2.45/月对应原价 $4.9/月的 5 折，活动页面日期是 2026-09-24。促销有时限，下单前在选购页确认当前是否还是这个价。

其他区域、其他配置和年付价格，官网公开页没有完整列出。想比价的话，直接进选购页看实际报价。

👉 [按区域查看 LightLayer 当前可选配置](https://account.lightlayer.net/?affid=74)

## DigitalOcean 到底哪里不够用

DigitalOcean 的定位是自助式 IaaS：服务器给你，操作系统以上全归你管。它的三个压力点是入门价、规模化之后的每美元算力、以及自管运维的工作量。具体到产品层面，有几处短板是已核验的。

Kubernetes 是差距最明显的一块。DOKS 提供托管控制面，worker 节点按标准 Droplet 计费，控制面本身不额外收费。它缺自动节点修复、节点自动扩缩容，也没有原生多集群网络和高级节点池配置。2025 年有过一次显著改进，但和 GKE、EKS、AKS 的工具链完整度比，仍有距离。Rackspace 自家博客也提到 DOKS 缺自动节点修复和自动供给。

App Platform 支持 Git 部署，但不支持金丝雀和蓝绿发布，高级流水线要外挂外部工具。可观测性方面，自带的是基础资源监控和 Managed OpenSearch，分布式追踪和跨集群观测要接第三方。BYOC 也没有完整方案：有 VPC 和 VPN，但没有集成式 BYOC。

规模化之后价格竞争力下降，高 CPU 配置对小型团队偏贵。单人项目受影响不大，跑几十台实例的团队会看到实打实的账单差异。

## 四条替代路线与已核验价格

同构 IaaS 换过去，要重新学的是控制台、API 命名和网络配置的位置，应用架构不用动。这是四条路线里迁移成本最低的一条。

### 同构 IaaS：Vultr、Linode、Hetzner

Vultr 的入门 Regular Performance 是 $2.50/月，0.5 GB 内存、10 GB SSD、1 vCPU、0.50 TB 月流量，小时价 $0.004。规格和 DO 入门 Droplet 几乎一一对应，月费和小时费都更低。它的弱点是高级托管服务少，想要托管数据库、托管 K8s 这类能力，选择面比 DO 窄。

Linode（Akamai Cloud）的 Nanode 入门是 $5.00/月，1 GB 内存、1 CPU、25 GB 存储、1 TB 月流量，小时价 $0.0075。比 DO 贵 1 美元，多出来的是内存和存储余量。它有一个 DO 没有的东西：按实例计费的托管加购，$100/实例/月，把打补丁、监控、事件响应外包出去，同时保留 IaaS 的计价模型。LKE 提供托管控制面，Akamai App Platform 标价 $0，底层 LKE 计算资源按用量计费。弱点是托管服务种类比超大规模厂商少。

Hetzner 的 CX23 是 $4.09/月，2 vCPU、4 GB 内存、40 GB NVMe、20 TB 月流量。它的产品线分四族：CX（成本优化共享 vCPU）、CPX（AMD EPYC 共享）、CAX（Arm64，最低价 €3.99/月起）、CCX（独占 vCPU，没有 noisy neighbor）。托管 Kubernetes（HKE）控制面免费，支持自动扩缩节点组和集成负载均衡。自有机房在 Nuremberg、Falkenstein、Helsinki、Ashburn、Hillsboro、Singapore。20 TB 月流量是竞品的 4–5 倍，高出口业务用 Hetzner，这一项比月费本身更能改变总成本。弱点是托管服务种类有限，规模扩大后自管工作量更多。

### 竞价与托管 K8s：Rackspace Spot

Rackspace Spot 的 spot 实例走公开市场拍卖，起拍 $0.001/hr，合 $0.72/月，同时提供 on-demand。Kubernetes 控制面免费，内置 Autoscaler，CNI 可选 Calico 或 Cilium，提供节点抢占预警。持久卷 SATA/SSD 起 $0.02/GB-月，Gen-2 机房高性能 NVMe 是 $0.06/GB-月。负载均衡 $10/月固定价，不收流量费。DBaaS 只支持托管 PostgreSQL。

Rackspace 自家博客宣称比 DigitalOcean 固定按需价最高省 87%，这个数字没有第三方验证，只能当厂商宣传看。它的机房数量比 DO 少，可选服务器规格受容量池限制。适合已经把工作负载容器化、能接受 spot 中断风险的团队。

### 托管平台：Cloudways、Northflank、Render

Cloudways 的 Autonomous Micro 是 $14/月，1 GB 内存、1 vCPU、1 TB 流量、网站数不限，小时价 $0.0208。它本质是在 DigitalOcean、Vultr、Linode、AWS、GCE 之上加一层托管平台，不是自有 IaaS。买的是服务器管理层，自动备份、staging 流程、控制面板，代价是低层基础设施控制力变弱。备份单独计费，$0.033/GB 已存数据。

Northflank 有免费额度，按量付费 $2.70/月起，支持 BYOC，内置 CI/CD 和托管 K8s，不提供传统 VPS。Render 有免费额度，是 Git 部署的 PaaS，基础设施控制力有限。

### 超大规模：AWS、GCP、Azure

三家都有免费额度，之后转按量付费。AWS 的托管服务目录和合规认证最广，GCP 的 GKE 最成熟、AI/ML 工具强，Azure 适合 Windows Server 和混合云。小团队用 Azure 可能偏贵，AWS 的复杂度和价格可预测性差。需要合规认证、海量托管服务、企业级 SLA 的团队，直接去这三家，不要在轻量云里绕。

### 对比表

| 品牌 | 入门价 | 给多少内存/存储/流量 | 最适合谁 |
| --- | --- | --- | --- |
| DigitalOcean | $4.00/月 | 512 MiB / 10 GiB / 500 GiB | 基准线，自助 IaaS |
| Vultr | $2.50/月 | 0.5 GB / 10 GB / 0.50 TB | 同管理模型下压月费 |
| Linode | $5.00/月 | 1 GB / 25 GB / 1 TB | 要更多余量，想留托管退路 |
| Hetzner CX23 | $4.09/月 | 4 GB / 40 GB NVMe / 20 TB | 欧洲业务、高出口流量 |
| Rackspace Spot | $0.001/hr 起 | 按拍卖配置 | 容器化、能接受 spot |
| Cloudways | $14/月 | 1 GB / 1 vCPU / 1 TB | 不想自己运维 |
| LightLayer SJ-Lite Cloud | $2.45/月（促销） | 1 GB / 50 GB HDD / 300M–500G 优化带宽 | 圣何塞轻量应用、预算敏感 |

## 真实总成本：备份、流量、年付

首年价不能直接比，有三项会改变总账。

备份计价方式不同。DigitalOcean 的自动备份按 Droplet 费用的固定百分比计价：Weekly 20%，Daily 30%，不按备份容量算。一个月费 $4 的 Droplet，开 Daily 备份就是每月多 $1.2，账单可预测。Cloudways 按 $0.033/GB 已存数据计费，备份体积越大账单越高。同样是“带备份”，两家的成本曲线不一样。

流量额度差异。Hetzner 的 EU 区给 20 TB/月，是 DO 入门 500 GiB 的几十倍。跑视频、镜像、下载站这类高出口业务，流量超额的费用可能比月费本身高。选 Hetzner 的理由里，20 TB 往往比 $4.09 的月费更值钱。

年付与月付的差额。年付通常有折扣，但折扣力度、续费价、退款政策各家不同。LightLayer 官网公开页没有完整列出年付价，下单前要在选购页确认年付单价、续费是否同价、以及退款条件。这条对任何厂商都适用。首年便宜不等于第二年便宜。

👉 [确认 LightLayer 年付价格与续费条件](https://account.lightlayer.net/?affid=74)

## 谁不该换

只需要 512 MiB 入门 Droplet、月费 $4 不介意的人，换厂商省下的钱可能不抵迁移和重新配置的时间。Vultr 每月省 $1.5，一年 $18，如果迁移要花半天，这笔账不划算。

需要合规认证、海量托管服务、企业级 SLA 的团队，直接去 AWS、GCP、Azure。轻量云厂商在这三项上给不了。

已经深度使用 DOKS、App Platform、Managed Databases 且没有明显痛点的团队，换过去要重新搭一套托管服务栈，迁移成本高于账单节省。

## 迁移六步

1. 盘点现有服务：实例、数据库、对象存储、DNS、证书、定时任务。
2. 选新厂商：按“卡在哪一项”选，不按“哪个最好”选。
3. 在新环境预置计算、网络、数据库、安全策略。
4. 迁移数据：导出导入、快照或厂商工具。
5. 功能测试与压力测试。
6. 改 DNS 切流量。

## 最后的选择规则

只需要同规格、更低月费：Vultr，$2.50/月。
需要更多内存和存储、想留托管退路：Linode，$5/月，托管加购 $100/实例/月。
欧洲业务、高出口流量：Hetzner CX23，$4.09/月含 20 TB。
不想自己运维：Cloudways，$14/月起。
需要 spot 算力加免费 K8s 控制面：Rackspace Spot，$0.001/hr 起。
圣何塞轻量应用、预算压在 $3 上下：LightLayer 的 SJ-Lite Cloud，促销 $2.45/月。

下单前确认两件事：机房位置是否靠近你的用户，计费周期和续费价是否和首期一致。LightLayer 的促销价有时限，其他家的入门价也会变。

👉 [查看 LightLayer 当前套餐与促销价](https://account.lightlayer.net/?affid=74)
