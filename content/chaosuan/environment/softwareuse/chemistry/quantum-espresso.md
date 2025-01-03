---
title: "Quantum ESPRESSO"
description: Test description
draft: false
enableToc: false
keyword: test
weight: 13
---

## Quantum ESPRESSO 是用于纳米级电子结构计算和材料建模的开源软件包

###### Quantum ESPRESSO基于密度泛函理论、平面波和赝势（范数守恒和超软）
***
#### 一、脚本模板
1. Slurm
    ```bash
    #!/bin/bash
    #SBATCH --nodes=1                   # 节点数量
    #SBATCH --ntasks-per-node=56        # 每个节点核心数量
    #SBATCH --ntasks=56                 # 总核心数
    #SBATCH --partition=g1_share        # 队列分区且必须指定正确分区
    #SBATCH --job-name=qe               # 作业名称
    #SBATCH --output=hello.%j.out       # 正常日志输出 (%j 参数值为 jobId)
    #SBATCH --error=hello.%j.err        # 错误日志输出 (%j 参数值为 jobId)

    ##############################################
    #          Software Envrironment             #
    ##############################################
    unset I_MPI_PMI_LIBRARY                        # 取消默认mpi库，使用intel自带
    export I_MPI_JOB_RESPECT_PROCESS_PLACEMENT=0   # intel 多节点作业所需修改参数
    module load intel/2022 intelmpi/2022           # intel 环境加载
    module load qe-6.2.1                           # 软件加载

    ##############################################
    #               Run job                      #
    ##############################################
    export OMP_NUM_THREADS=1
    ulimit -s unlimited
    ulimit -l unlimited

    mpirun pw.x -i ausurf.in
    ```

***

#### 二、编译
1. 环境加载

    ```bash
    #intel编译
    module load intelmpi/2022 intel/2022
    #gcc编译
    module load gcc/12.1.0 openmpi/4.1.3_gcc12.1.0
    ```

2. QE安装

    ```bash
    #QE下载链接: https://github.com/QEF/q-e/releases/tag/qe-7.0
    #Devicexlib下载链接: https://gitlab.com/max-centre/components/devicexlib/-/archive/0.1.0/devicexlib-0.1.0.tar.gz
    tar -xf qe.tar.gz
    cd q-e-qe-7.0/
    ./configure --prefix=/sh2/software/app/qe
    cp -r devicexlib-0.1.0/* /sh2/software/app/qe/q-e-qe-7.0/external/devxlib
    cd q-e-qe-7.0/
    make all install
    ```

###### ERROR:  系统试图使用wget命令从gitlab中下载一个名为devicexlib-0.1.0.tar.gz的外部包，并将其解压到./external/devxlib中。如果用户所在的系统未配置外部互联网，则无法下载成功，紧接着的tar命令自然找不到文件，也无法编译安装devicelib，进而整个make安装过程自然报错退出。

###### 解决方案：手动从提示的链接下载devicexlib包并且解压到$QEPATH/external/devxlib中，然后重新回到QE主目录$QEPATH执行正常安装即可。
