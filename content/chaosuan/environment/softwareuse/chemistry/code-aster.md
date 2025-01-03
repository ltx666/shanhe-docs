---
title: "CodeAster"
description: Test description
draft: false
enableToc: false
keyword: test
weight: 3
---
## Code Aster

###### Code Aster是法国EDF集团研发中心开发的一款基于有限元方法的结构分析软件，主要用于3D热、静力学、结构力学、材料力学以及线性及非线性动力学分析。可编程、易扩展，已形成一定的用户群，广泛用于核电站结构的分析和设计。

***
#### 一、脚本模板
1. Slurm
    ```bash
    #!/bin/bash
    #SBATCH --nodes=1                   # 节点数量
    #SBATCH --ntasks-per-node=56        # 每个节点核心数量
    #SBATCH --ntasks=56                 # 总核心数
    #SBATCH --partition=g1_share        # 队列分区且必须指定正确分区
    #SBATCH --job-name=vasp             # 作业名称
    #SBATCH --output=vasp.%j.out        # 正常日志输出 (%j 参数值为 jobId)
    #SBATCH --error=vasp.%j.err         # 错误日志输出 (%j 参数值为 jobId)

    ##############################################
    #          Software Envrironment             #
    ##############################################
    unset I_MPI_PMI_LIBRARY                        # 取消默认mpi库，使用intel自带
    export I_MPI_JOB_RESPECT_PROCESS_PLACEMENT=0   # intel 多节点作业所需修改参数
    # load intel/2022 intelmpi/2022           # intel 环境加载

    ##############################################
    #               Run job                      #
    ##############################################
    export OMP_NUM_THREADS=1

    as_run --test forma01a
    ```

***

#### 二、软件安装

1. 依赖环境
软件安装依赖于python3环境，安装前首先配置好python3环境以及软件numpy sphinx包。

```bash
module load conda3
conda create -n python3 python=3
conda install numpy sphinx
```

2. 进入软件目录安装

    ```bash
    python3 setup.py install --prefix=/es01/software/apps/aster-full-src-14.6.0/

    # 同目录下有个配置文件setup.cfg，默认使用GNU编译器，想用Intel编译器的可以根据内部注释的提示修改
    ```
