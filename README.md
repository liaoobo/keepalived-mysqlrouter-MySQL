# 基于keepalived+mysqlrouter+gtid半同步复制的MySQL集群

## 1.拓扑图

![请添加图片描述](https://img-blog.csdnimg.cn/c23faf7230ef46fb8d0d4c692c891885.png)



## 2.详细介绍

项目名称：基于keepalived+mysqlrouter+gtid半同步复制的MySQL集群

项目环境：centos7.9，mysql5.7.30，mysqlrouter8.0.21，keepalived 1.3.5，ansible 2.9.27等

项目描述：
     本项目的目的是构建一个高可用的能实现读写分离的高效的MySQL集群，确保业务的稳定，能沟通方便的监控整个集群，同时能批量的去部署和管理整个集群。

项目步骤：

	1.配置好ansible服务器并建立免密通道，一键安装MySQL、mysqlroute、node_exporters、dns等软件，在master上导出基础数据到ansible上，发布到所有slave服务器上并导入
 
	2.安装好半同步相关的插件，开启gtid功能，启动主从复制服务，配置延迟备份服务器，从slave1上拿二进制日志
 
	3.在master上创建一个计划任务每天2:30进行数据库的备份脚本，使用rsync+sersync远程同步到slave4异地备份服务器上
 
	4.安装部署mysqlrouter中间件软件，实现读写分离；安装keepalived实现高可用，配置2个vrrp实例实现双vip的高可用功能
 
	5.搭建DNS域名服务器，配置一个域名对应2个vip，实现基于DNS的负载均衡，访问同一URL解析出双vip地址
 
	6.使用sysbench整个MySQL集群的性能（cpu、IO、内存等）进行压力测试，安装部署prometheus实现监控，grafana出图了解系统性能的瓶颈并调优

项目心得：

    1.一定要规划好整个集群的架构，配置要细心，脚本要提前准备好，边做边修改，防火墙和selinux的问题一定要多注意

    2.对MySQL的集群和高可用有了深入的理解，对自动化批量部署和监控有了更加多的应用和理解

    3.keepalived的配置需要更加细心，对keepalievd的脑裂和vip漂移现象也有了更加深刻的体会和分析

    4.认识到了系统性能资源的重要性，对压力测试下整个集群的瓶颈有了一个整体概念
