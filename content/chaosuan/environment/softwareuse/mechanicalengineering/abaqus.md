---
title: "ABAQUS"
description: Test description
draft: false
enableToc: false
keyword: test
weight: 13
---

## ABAQUS 是一种有限元素法软件

###### 用于机械、土木、电子等行业的结构和场分析。

###### 请自行购买ABAQUS license许可，下载和安装。如需协助安装或使用，请联系我们，附上课题组拥有ABAQUS license的证明。

***
#### 一、脚本模板
1. Slurm

    ```bash
    #!/bin/bash
    #SBATCH --nodes=1                   # 节点数量
    #SBATCH --ntasks-per-node=56        # 每个节点核心数量
    #SBATCH --ntasks=56                 # 总核心数
    #SBATCH --partition=g1_share        # 队列分区且必须指定正确分区
    #SBATCH --job-name=abaqus    	    # 作业名称
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
    ulimit -s unlimited
    ulimit -l unlimited
    nameflag=`scontrol show hostname $SLURM_JOB_NODELIST`
    beginflag="mp_host_list=["
    endflag="]"
    for line in $nameflag
    do
    tempflag="['$line',56],"
    beginflag=$beginflag$tempflag
    done
    eflag=`echo $beginflag | sed 's/.\{0\}.$//'`
    endflag=$eflag$endflag
    echo $endflag > abaqus_v6.env

    # JOBNAME=`ls | grep inp | sed 's/.\{3\}.$//'`
    JOBNAME=test_test

    # For program
    #USERNAME=huang.for

    # change scratch to the actual directory
    #abaqus job=${JOBNAME} user=${USERNAME} input=${JOBNAME}.inp globalmodel=zd.odb cpus=$SLURM_NPROCS scratch=./ interactive

    abaqus job=${JOBNAME} input=${JOBNAME}.inp analysis cpus=$SLURM_NPROCS scratch=$SLURM_SUBMIT_DIR interactive
    ```

***

#### 二、安装
