---
title: "OPTOSTRUCT"
description: Test description
draft: false
enableToc: false
keyword: test
weight: 13
---

## Optistruct

######  Altair OptiStruct是业界公认的功能最强的结构分析及优化求解器，可用来分析静态和动态载荷条件下的线性和非线性结构问题。作为结构设计和优化的市场领导者，OptiStruct帮助设计者和工程师分析和优化结构的应力、耐久性和NVH（噪声、振动和舒适度）的特性并快速研发创新、轻量化的高效的结构设计。

###### 请自行购买Optistruct license许可，下载和安装。如需协助安装或使用，请联系我们，附上课题组拥有Optistruct license的证明。

***
#### 一、脚本模板
1. Slurm
    ```bash
    #!/bin/bash
    #SBATCH --nodes=1                   # 节点数量
    #SBATCH --ntasks-per-node=56        # 每个节点核心数量
    #SBATCH --ntasks=56                 # 总核心数
    #SBATCH --partition=g1_share        # 队列分区且必须指定正确分区
    #SBATCH --job-name=optistruct       # 作业名称
    #SBATCH --output=hello.%j.out       # 正常日志输出 (%j 参数值为 jobId)
    #SBATCH --error=hello.%j.err        # 错误日志输出 (%j 参数值为 jobId)

    ##############################################
    #          Software Envrironment             #
    ##############################################
    unset I_MPI_PMI_LIBRARY                        # 取消默认mpi库，使用intel自带
    export I_MPI_JOB_RESPECT_PROCESS_PLACEMENT=0   # intel 多节点作业所需修改参数
    # module load intel/2022 intelmpi/2022           # intel 环境加载

    ##############################################
    #               Run job                      #
    ##############################################

    #mpirun -np $SLURM_NPROCS optistruct_2017.0_linux64_impi 420-DG-ncode.dat -ddmmode
    optistruct 420-DG-ncode.dat -mpi -ddm -core IN -np 7 -nt 8

    ```

***

#### 二、安装
