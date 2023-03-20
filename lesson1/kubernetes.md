`Kubernetes`是一款由Google开源的容器编排管理工具
“云计算”概念由Google提出

k8s的目标是消除编排物理或者虚拟计算、网络和存储等基础设施的负担


# k8s的架构设计

Borg架构：
Borg使用 Cell 来定义一组机器资源，Google内部一个中等规模的 Cell 可以管理 1万 台左右服务器，这些服务器的配置可以是异构的
Cluster 即集群，一个数据中心可以同时运行一个或者多个集群，每个集群又可以有多个Cell

## k8s的组件
1. kube-apiserver
   是整个集群的“灵魂”，是信息的汇聚中枢，提供了所有内部和外部的API请求操作的唯一入口
   同时也负责整个集群的认证、授权、访问控制、服务发现等能力
2. kube-controller-manager
   负责维护整个集群的状态，比如多副本创建、滚动更新等
3. kube-scheduler
   监听未调度的Pod，按照预定的调度策略绑定到满足条件的节点上


Master 和 Node 的交互方式：
k8s中所有的状态都是采用*上报*的方式实现的，APIServer 不会主动跟 Kubelet 建立请求链接
所有的容器状态汇报都是由 Kubelet 主动向 APIServer 发起的
新增的 Node 被 APIServer 纳管后，Kubelet 进程就会定时向 APIServer 汇报“心跳”，即汇报自身状态，包括自身健康状态，负载数据统计等
当一段时间内心跳包没有更新，此时 kube-controller-manager 就会将其标记为`NodeLost(失联)`
