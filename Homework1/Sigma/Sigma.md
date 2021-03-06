# 阿里巴巴 Sigma 调度和集群管理系统架构学习报告
## 简介
Sigma 是服务阿里巴巴在线业务的Pouch容器调度系统。Sigma-cerebro 系统是 Sigma 系统的调度模拟系统，在2017年双11中，依靠  
cerebro 进行预处理，Sigma 成功完成了双11一键建站，30分钟内完成建站任务，且系统静态分配率从66%提升到95%，大大提升了资源利用的有效性。
## 主要特征
### 统一调度体系
Sigma 由 Alikenel、SigmaSlave、SigmaMaster 三层大脑联动协作。  
* Alikenel 部署在每一台物理机上，对内核进行增强，在资源分配、时间片分配上进行灵活的按优先级和策略调整，对任务的时延，任务时间片的抢占、不合理抢占的驱逐都能通过上层的规则配置自行决策。
* SigmaSlave 可以在本机进行容器 CPU 分配、应急场景处理等。通过本机 Slave 对时延敏感任务的干扰快速做出决策和响应，避免因全局决策处理时间长带来的业务损失。
* SigmaMaster 是一个最强的中心大脑，可以统揽全局，为大量物理机的容器部署进行资源调度分配和算法优化决策。

整个架构是面向终态的设计理念，收到请求后把数据存储到持久化存储层，调度器识别调度需求分配资源位置。

### 混部架构
Sigma 通过 SigmaAgent 启动 PouchContainer 容器。Fuxi 也在这台物理机上抢占资源，启动自己的计算任务。所有在线任务都在 PouchContainer 容器上，它负责
把服务器资源进行分配并运行在线任务，离线任务填入其空白区，保证物理机资源利用达到饱和，从而完成了两种任务的混合部署。

### 关键技术
* 全面容器化技术
* 分时复用技术
* 内核资源隔离技术
* 在线集群管理技术

## 利&弊
### 优点
* 隔离性好  
Sigma的容器系统PouchContainer是富容器，可以登录，看到容器内进程自己占的资源量，有多少进程。此外还保证了系统的稳定性，进程的状态不会影响到容器的正常运行。
* 兼容性好  
支持旧版本内核，能够更好地利用资源。同时兼容了业界更多标准，推动标准的建设，支持 RunC 、RunV 、RunLXC 等。
* 结构清晰  
Sigma在存储方面，跟业界一起建设了 CSI 标准。同时支持分布式存储如 ceph、pangu。网络上，其使用 了lxcfs 增强隔离性，并支持多种标准。

### 缺点
* 自动化容器的部署和复制还不够健全
* 没有提供足够的容器弹性，容器失效时没有健全的方法去替换它

## 个人感想
* 企业进行架构升级和创新要具有前瞻性  
阿里巴巴早在2011年（此时双11还未发展壮大），就开始进行调度管理系统的研发，充分体现了其前瞻性。在之后对其进行上线，在企业最需要的情况下成功地进行了调度
管理系统的升级，确保了业务平稳发展。
* 云化架构是现今调度分配架构的潮流  
在大规模业务处理的要求下，云化架构运维体系是一个重要的技术解决方案。利用规模化运维系统在容器上构建大量在线服务可以带来切实的系统收益和利用率。
