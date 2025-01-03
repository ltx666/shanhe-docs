---
title: "LSDYNA"
description: Test description
draft: false
enableToc: false
keyword: test
weight: 13
---

## LSDYNA

###### LS-DYNA是由前 Livermore Software Technology Corporation (LSTC) 开发的高级通用多物理场仿真软件包，该公司于 2019年被Ansys收购。 该软件包继续包含越来越多的计算可能性许多复杂的现实世界问题，其起源和核心竞争力在于使用显式时间积分的高度非线性瞬态动态有限元分析(FEA)。LS-DYNA用于汽车、航空航天、建筑和土木工程、军事、制造和生物工程行业。

###### 请自行购买LSDYNA license许可，下载和安装。如需协助安装或使用，请联系我们，附上课题组拥有LSDYNA license的证明。

***
#### 一、脚本模板
1. Slurm

    ```bash
    #!/bin/bash
    #SBATCH --nodes=1                   # 节点数量
    #SBATCH --ntasks-per-node=56        # 每个节点核心数量
    #SBATCH --ntasks=56                 # 总核心数
    #SBATCH --partition=g1_share        # 队列分区且必须指定正确分区
    #SBATCH --job-name=dyna     	    # 作业名称
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
    nameflag=`scontrol show hostname $SLURM_JOB_NODELIST`
    for line in $nameflag
    do
    tempflag="$line:56:"
    beginflag=$beginflag$tempflag
    done
    eflag=`echo $beginflag | sed 's/.\{0\}.$//'`
    endflag=$eflag$endflag
    echo $endflag > test.log

    # JOBNAME=`ls | grep inp | sed 's/.\{3\}.$//'`
    JOBNAME=000_Master.k
    # change scratch to the actual directory
    lsdyna -dis -machines $endflag i=$JOBNAME memory=800m memory2=200m
    # mpirun  -hosts `scontrol show hostname $SLURM_JOB_NODELIST |tr '\n' ',' | head -c-1` lsdyna_dp_mpp.e   i=$JOBNAME memory=1200m memory2=400m
    ```

***

#### 二、安装
