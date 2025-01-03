---
title: "CFX"
description: Test description
draft: false
enableToc: false
keyword: test
weight: 13
---

## CFX

###### ANSYS CFX作为世界上唯一采用全隐式耦合算法的大型商业软件，算法上的先进性，丰富的物理模型和前后处理的完善性使其在结果精确性、计算稳定性、计算速度和灵活性上都有优异的表现。除了一般工业流动以外，CFX还可以模拟诸如燃烧，多相流，化学反应等复杂流场。集成到ANSYS Workbench环境中使用，增加了在工程仿真的应用面，效率达到新的水平。

###### 请自行购买CFX license许可，下载和安装。如需协助安装或使用，请联系我们，附上课题组拥有CFX license的证明。

***
#### 一、脚本模板
1. Slurm
    ```bash
    #!/bin/bash
    #SBATCH --nodes=1                   # 节点数量
    #SBATCH --ntasks-per-node=56        # 每个节点核心数量
    #SBATCH --ntasks=56                 # 总核心数
    #SBATCH --partition=g1_share        # 队列分区且必须指定正确分区
    #SBATCH --job-name=cfx              # 作业名称
    #SBATCH --output=hello.%j.out       # 正常日志输出 (%j 参数值为 jobId)
    #SBATCH --error=hello.%j.err        # 错误日志输出 (%j 参数值为 jobId)

    ##############################################
    #          Software Envrironment             #
    ##############################################
    #unset I_MPI_PMI_LIBRARY                        # 取消默认mpi库，使用intel自带
    #export I_MPI_JOB_RESPECT_PROCESS_PLACEMENT=0   # intel 多节点作业所需修改参数
    # module load intel/2022 intelmpi/2022           # intel 环境加载

    ##############################################
    #               Run job                      #
    ##############################################
    #Generate line of names of computational nodes (or hosts).
    MYHOSTLIST=$(srun hostname | sort | \
    uniq -c | \
    awk '{print $2 "*" $1}' | \
    paste -sd, -)
    echo $MYHOSTLIST
    #Run Ansys CFX.
    cfx5solve -def inputfile.def -parallel -start-method "Intel MPI Distributed Parallel" -par-dist "$MYHOSTLIST" -batch
    ```

***

#### 二、安装
