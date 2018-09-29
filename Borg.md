Borg Microsoft
================
> ***Large-scale cluster management of Google*** 

---
Introduction
==========
##### Google的Borg系统是一个集群管理工具，它调度，启动，重启以及监视Google运行的所有应用。
##### 在它上面运行着成千上万的job，这些job来自许许多多不同的应用，并且跨越多个集群，而每个集群又由大量的机器构成。
![70%](Borg.png)

---
Related Concept
===============
### 1. Cell，Job和Task 
>   - Job由一个或多个Task组成
>   - Cell是包括了多台机器的单元
>   - 用户以Job的方式提交他们的工作给Borg，一个Job只能在一个Borg的Cell里面跑
    
### 2. Borg architecture 
>   - Borgmaster：整个Borg系统逻辑上的中心节点
>   - Borglet：部署在所有服务器上的Agent，负责接收Borgmaster进程的指令
  
---
Characteristics
===============
## 1. Scalability 
	> Borgmaster主进程有5个副本
    > scheduler进程也是多副本运行的设计，一主多从
	> Borgmaster采用轮询的方式定期从Borglet获取该节点的状态

## 2. Availability
	> 即使Borgmaster或者Borglet挂了，task也会继续运行下去
    > 每个Cell的Borg均独立部署，避免不同Borg系统相互影响
	> 部署实例时用简单、底层的工具去减少外部依赖
---	
Characteristics
===============

## 3. Utilization 
	> 98%的服务器实现了混部
    > 90%的服务器中跑了超过25个Task和4500个线程
	> 在一个中等规模的Cell里，在线任务和离线任务独立部署比混合部署所需
	  的服务器数量多出约20%-30%
## 4. Isolation
	> 安全性隔离：采用chroot jail实现
    > 性能隔离：采用基于cgroup的容器技术实现。
      >- 通过不同优先级之间的抢占式调度来优先保障在线任务的性能，牺牲
      离线任务
      >- 当compressible类型的资源成为瓶颈时，Borg不会Kill掉相应的任务
      而当non-compressible类型的资源成为瓶颈时，Borg会Kill掉相应的任
      务

---
 
Pros & Cons
===========
---
Pons
====
> + 隐藏资源管理和故障处理细节，使其用户可以专注于应用开发
> + 高可靠性和高可用性的操作，并支持应用程序做到高可靠高可用
> + 可在数以万计的机器上有效运行
---
Cons
====
> + Jobs是唯一的task分组的机制
> 		- 语义僵硬，无法滚动升级和改变job的实例数
>+ 一台机器上的所有task都使用同一个IP地址，共享端口空间
>		- Borg必须把端口当做资源来调度
>		- task必须先声明他们需要多少端口
>+ 复杂的API让初级用户使用困难
---
 
My Comments
===========
Borg从经济效益上来说每年帮助Google节省了大笔服务器的开销
Borg从应用角度来说虽不是第一个类似的系统，但却是少数几个可以在这么大的规模上运行且具有高弹性和完整性的
Kubernetes将Borg最精华的部分提取出来，使现在的开发者能够更简单、直接地应用，正快速发展成长为容器管理领域的领导者

