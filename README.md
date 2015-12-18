### NodeJS服务监控报警系统的核心实现和开源共建

1. 建设服务监控系统的初衷和历程
2. 核心实现思路及实际应用
3. 现有功能点介绍和路线图
4. 代码半开源的目的、愿景
5. 开源代码的结构和部署拓扑图
6. 项目开发者的个人介绍

#### 建设服务监控系统的初衷和历程

随着基于NodeJS前后端分离方案的推行，前端的开发模式和角色也在发生着悄无声息的变化，而今NodeJS的开发俨然已经成为我们日常工作中的一部分，前端工程师与服务端、运维都有了更多的交集，但随着业务和项目的扩张，生产环境Node服务也在不断增多，如何对这些服务的运行状态和各项指标了如指掌是当前我们大家共同遇到的挑战。

我的初衷是建立一个专门针对NodeJS的服务监控平台，它能够支持服务集群的统一管理和多用户登录，旨在帮助团队的开发者、架构人员以及管理者能够直观的观察线上各个服务的实时状态，并能够帮助开发者及时发现线上服务的异常情况（比如内存泄露、服务崩溃重启、慢路由等）。

PM2是一款非常优秀的Node进程管理工具，它有着丰富的特性：能够充分利用多核CPU且能够负载均衡、能够帮助应用在崩溃后自动重启、能够监控资源的使用情况并且支持API方式查看、并且有配套的Keymetrics可以用于服务的监控。相信不少开发者都在使用PM2部署自己的NodeJS应用，起初也曾尝试使用Keymetrics，但Keymetrics是一款商业服务且价格不菲，虽有两台服务器的免费配额但对于有着众多服务器的团队或者公司而言无异于杯水车薪，而且对于国内的大公司而言都会考虑到自身的数据敏感性不希望服务的运行状态接入到第三方平台中，当然如果你的服务器数量很少，又或者能够支付昂贵的使用费用，而且无需关心数据的安全问题依旧推荐可以使用Keymetrics，毕竟它是PM2的开发者的开发和维护，而且功能特性也很丰富。

但我遇到的场景是如何实现一套通用的监控系统，它能够独立部署而且方便进行扩展和定制化，显然在现阶段使用Keymetrics不是十分明智的决定，于是便想到利用PM2的API进行服务监控，但API的方式不利于多台机器的统一管理和聚合，而且通过API的方式会将服务的详细信息泄露到外网，有一定的安全隐患。之后便开始设想是否能够基于PM2进行一些二次开发，实现一个核心功能并具备一定定制性的系统（类似Keymetrics），一方面可以为整个行业的Node推广起到一定的积极作用，一方面能够确实帮助到我们团队的服务运维。

#### 现有功能点介绍和路线图

- 支持用户的管理（登录作为中间件）
- 被监控机器的分桶管理
- 机器列表、快速过滤和主机的指标信息（进程数、CPU数、负载、上线时长、内存占用）
- 进程的详细指标信息（PID、进程名、重启次数、上线时长、状态、CPU占用、内存占用、错误日志）
- 同Falcon整合，支持监控报警管理（核心指标同步Falcon，可以查看历史图表或者配置监控报警）
- 支持扩展包，引入扩展包后可以收集统计服务端慢路由信息
- 支持进程的远程控制，可以在云端对进程进行远程操作（比如重启、重载）

#### 代码开源的目的、愿景

我的初衷很简单，一方面能够把自己在工作之余的贡献拿出来和行业分享，也希望绵薄之力能够对行业的Node推广起到一些作用，反过来也希望行业内的济济人才能够参与到它的建设中来，基于我目前的成果和代码进行更多的功能开发和细节完善，把大家对Node的热忱和对PM25的建设形成合力为行业带来一些影响或是价值。

#### 开源代码的组成结构

```
├── PM2.5-Cli           // PM25 CLI
├── PM2.5-Cloud         // PM25 云端控制台
├── PM2.5-Service       // PM25 服务端代码
├── PM2.5-Docs          // PM25 文档
└── PM2.5-Ext           // PM25 扩展包
```

#### 部署拓扑图

![](http://ww1.sinaimg.cn/large/62755f82jw1ez3ueeghpvj20hu0g0acc.jpg)

#### 项目作者的个人背景介绍

郭凯，工作狂、强迫症，崇尚工匠精神，全栈工程师，翻译作品有《编写可维护的JavaScript》、《第三方JavaScript编程》。开源项目有In、Juicer、jSQL、以及开源前端技术社区F2E等。

