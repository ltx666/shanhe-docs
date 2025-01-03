---
title: "slurm介绍"
description: Test description
draft: false
enableToc: false
keyword: test
weight: 2
---
## Slurm任务调度工具

![](/chaosuan/submitjob/_image/slurm.png)


Slurm 任务调度工具（前身为极简Linux资源管理工具，英文：Simple Linux Utility for Resource Management，取首字母，简写为SLURM），或简称为Slurm，是一个用于 Linux 和 Unix 内核系统的免费、开源的任务调度工具，被世界上许多超级计算机和计算机集群广泛采用。Slurm 使用基于希尔伯特曲线调度或胖树网络拓扑的最佳拟合算法，以优化并行计算机上任务分配的局部性。

**它提供了三个关键功能：**
- 第一，为用户分配一定时间的专享或非专享的资源(计算机节点)，以供用户执行工作。
- 第二，它提供了一个框架，用于启动、执行、监测在节点上运行着的任务(通常是并行的任务，例如 MPI)。
- 第三，为任务队列合理地分配资源。

***

**本集群采用slurm作业调度系统。**
