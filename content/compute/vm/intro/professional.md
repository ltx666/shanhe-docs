---
title: "专业级云服务器"
date: 2020-01-30T00:40:25+09:00
description: Test description
draft: false
enableToc: false
weight: 20
keyword: 云服务器, 实例, 虚拟机
---

## GPU云服务器 g2

具备 GPU 加速计算能力的实例，可同时提供 GPU 和 CPU 计算资源，大幅提高机器学习及科学计算等大规模计算框架的运行速度，为搭建人工智能及高性能计算平台提供基础架构支持。主要应用场景包括人工智能深度学习、高性能计算、图形图像处理等领域。

> **说明：**
>
> 仅在山河计算平台可创建弹性裸金属服务器。

特点：

- 三种GPU类型可选，专门为计算加速场景优化设计
  - NVIDIA A100针对 AI、数据分析和 HPC 应用场景，在不同规模下实现出色的加速，有效助力更高性能的弹性数据中心。A100 采用 NVIDIA Ampere 架构，提供超快速的显存带宽，可处理超大型模型和数据集。
  - NVIDIA RTX6000提供4608个并行计算核心，单精度浮点运算能力达16.3TFLOPS。
  - AMD FirePro S7150FirePro S7150采用了一颗28nm Tonga XT核心，拥有2048个流处理器，单精度浮点运算性能为3.77 TFLOPS，双精度浮点运算性能为250 GFLOPS。

- 两种CPU类型可选：
  - Intel Xeon(Ice Lake) Platinum 8358处理器，主频：2.6GHz，睿频最高可达3.4GHz
  - Intel Xeon Gold 5218处理器，主频：2.3GHz，睿频最高可达3.9GHz

- 配有全新的 Intel Advanced Vector Extension (AVX-512) 指令集，在深度学习的多种场景性能提升4倍
- 处理器内存配比1:4
- I/O优化
- 支持 VPC 私有网络和基础网络
- 系统盘支持企业级SSD硬盘，数据盘支持企业级SSD硬盘、容量型和企业级分布式 SAN

GPU云服务器 g2 包括的实例规格和性能指标如下：（内网带宽和显存待确定）

| 实例类型               | CPU  | 内存 | GPU                 | 内网带宽  |
| ---------------------- | ---- | ---- | -------------------  | ----------|
| g2as7150.16xlarge.g4	| 64核  | 192G  | 4*AMD FirePro S7150	| 50 Gbps		|
| g2as7150.4xlarge.g1		| 16核	| 48G		| 1*AMD FirePro S7150	| 12.5 Gbps	|
| g2as7150.8xlarge.g2		| 32核	| 96G		| 2*AMD FirePro S7150	| 25 Gbps   |
| g2na100.4xlarge.g1		| 16核	| 48G 	| 1*NVIDIA A100	    	| 2 Gbps   	|
| g2ibna100.4xlarge.g1	| 16核 	| 48G		| 1*NVIDIA A100	    	| 12.5 Gbps |
| g2na100.16xlarge.g4		| 64核 	|	192G	| 4*NVIDIA A100	   		| 8 Gbps   	|
| g2na100.31xlarge.g8		| 124核 |	432G	| 8*NVIDIA A100	 			| 20 Gbps 	|
| g2ibna100.16xlarge.g4	| 64核	|	192G	| 4*NVIDIA A100	 			| 50 Gbps   |
| g2na100.31xlarge.g6		| 120核 |	288G	| 6*NVIDIA A100	 			| 20 Gbps   |
| g2ibna100.31xlarge.g8	| 124核 |	432G	| 8*NVIDIA A100	  		| 100 Gbps  |
| g2ibna100.8xlarge.g2	| 32核	| 96G		| 2*NVIDIA A100	  		| 25 Gbps   |
| g2na100.8xlarge.g2		| 32核	| 96G		| 2*NVIDIA A100	    	| 4 Gbps    |
| g2nrtx6000.16xlarge.g2| 128核	| 384G	| 2*NVIDIA RTX6000	  | 50 Gbps   |
| g2nrtx6000.8xlarge.g1	| 64核	| 192G	| 1*NVIDIA RTX6000	  | 25 Gbps   |
| g2nt4.31xlarge.g6			| 120核	| 288G	| 6*NVIDIA Tesla T4	  | 20 Gbps   |
| g2nt4.16xlarge.g4			| 64核	| 192G	| 4*NVIDIA Tesla T4		| 8 Gbps 		|
| g2nt4.31xlarge.g8			| 124核	| 384G	| 8*NVIDIA Tesla T4		| 20 Gbps   |
| g2nt4.4xlarge.g1			| 16核	| 48G		| 1*NVIDIA Tesla T4		| 2 Gbps   	|
| g2nt4.8xlarge.g2			| 32核	| 96G 	|	2*NVIDIA Tesla T4		| 4 Gbps   	|



## 弹性裸金属云服务器 bm1

采用Intel Xeon Gold 5218处理器，提供高性能、资源独享、安全隔离的专属弹性裸金属云服务器群组，满足各类核心应用对高性能及稳定性的需求，同时提供完整的设备管理权限及运维服务。 用户可以像使用其他云资源一样，快速、灵活的部署及管理弹性裸金属云服务器，并可弹性购买。

> **说明：**
>
> 仅在山河计算平台可创建弹性裸金属服务器。

特点：

- Intel Xeon Gold 5218处理器，主频：2.3GHz，睿频最高可达3.9GHz

- 配有全新的 Intel Advanced Vector Extension (AVX-512) 指令集，在深度学习的多种场景性能提升4倍

- I/O优化

- 承载核心业务

  独享物理隔离的硬件资源，释放极致性能，匹配 VPC 网络赋予的完整控制能力， 满足核心业务对性能、可靠性及安全合规的苛刻需求求。

- 高效的资源交付

  10 分钟完成 OS 部署上线，自动化配置，秒级操作响应，兼容所有虚拟主机系统镜像及用户自有镜像，助您轻松完成基础架构部署。

- 灵活简便的使用体验

  VPC 直连，提供与虚拟主机（VM）一致的使用方式，支持弹性伸缩、弹性秒级计费及 API 自动化管理，赋予物理主机弹性、灵活与敏捷的云端特性。

| 实例类型        | CPU  | 内存 | 内网带宽 |
| :-------------- | :--- | :--- | :------- |
| bm1.16xlarge.r8 | 64核 | 512G | 50 Gbps  |

## 弹性裸金属云服务器 bm3 

采用Intel Xeon Gold 6258R处理器和 DPU 架构，通过引入智能网卡，实现存储、网络等数据处理功能卸载，让用户可**独享 CPU 计算资源**，实现裸金属服务器的完整算力保留，以极致性能满足各类核心业务需求。

弹性裸金属 BM3 **同时具备云服务器的弹性、敏捷性、灵活性与高可用**，支持按需使用，且用户业务应用可直接访问处理器和内存，无任何虚拟化开销。

> **说明：**
>
> 仅在山河计算平台可创建弹性裸金属服务器。

特点：

- Intel Xeon Gold 6258R处理器，主频：2.7GHz，睿频：4.0GHz
- 弹性裸金属 BM3具有112 核、512 G内存的超强算力
- 通过在服务器侧引入智能网卡，将网络、存储、操作系统中不适合由 CPU 支持的高性能数据处理功能卸载到智能网卡中执行
- I/O优化
- 支持 VPC 私有网络和基础网络
- 系统盘支持企业级SSD硬盘，数据盘支持企业级SSD硬盘、容量型和企业级分布式 SAN

| 实例类型        | CPU   | 内存 | 内网带宽 |
| :-------------- | :---- | :--- | :------- |
| bm3.16xlarge.r4 | 112核 | 512G | 50 Gbps  |

