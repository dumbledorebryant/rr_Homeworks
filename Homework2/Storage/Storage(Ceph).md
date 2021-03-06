# Ceph分布式文件系统学习报告
## 技术介绍
### 特点
* 解耦数据与元数据——Ceph最大化地将文件元数据管理和文件数据存储分离
* 动态分布式元数据管理——Ceph使用基于动态子树分离的元数据集群架构自适应地，智能地分配负责管理文  
件系统目录
* 可靠的自主分布式对象存储——Ceph将数据迁移，数据复制，错误检测，错误恢复的责任分配给存储数据的  
OSD集群。在更高一级别层次上，OSD共同提供单个逻辑对象存储到客户端和元数据服务器。
### 利与弊
#### 利
提高了文件系统的可扩展性，简化了系统的设计，减轻元数据集群的负载；提高了MDS资源的利用效率以提高  
系统性能；在每个OSD上实现了可靠的高可用对象存储。
#### 弊
* 在用户空间中重载系统调用会引发一系列问题，使得内核客户端模块不可避免。
* 安全架构和协议尚未实现
* 线性缩放仍然不够完美
### 取舍
总吞吐量和平衡元数据分布的取舍，有时不平衡的元数据分布可以带来更大的总吞吐量，但会带来性能上的  
降低。
## 关键指标
### I/O吞吐量
通过I/O不同大小的数据并检测OSD的相应的数据吞吐量。
### 写延迟
通过检测不同数据的写入和复制所需要的写延迟
### 数据分发和可伸缩性
通过检测不同大小的OSD集群规模及其对应的每个OSD的吞吐量来测量数据分发和可伸缩性。
### 元数据更新延迟
通过检测一定数量元数据复制所需的更新延迟及常规文件系统操作的累计元数据更新消耗时间来测量元数据更  
新延迟
### 元数据读延迟
通过检测一定数量的MDS集群规模对应的每个MDS吞吐量来测量元数据读延迟
### 元数据扩展
通过检测每个MDS吞吐量及其对应的延迟来测量元数据扩展性能
## 感想及评论
Ceph是一种新颖且高效的文件系统，将Ceph的技术大量运用于现今的文件系统会大幅度提高性能及吞吐量及  
存储设备使用效率。然而由于其需要新的POSIX I/O接口的实现，将其大量运用还需要一定时间。同时，Ceph  
也为分布式文件系统提供了一种可靠高性能的实现灵感。
