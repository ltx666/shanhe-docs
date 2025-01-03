---
title: "OCTOPUS"
description: Test description
draft: false
enableToc: false
keyword: test
weight: 11
---
## OCTOPUS
###### Octopus是一款赝势实空间软件包，用于含时电磁场下的一、二及三维有限系统的电子-离子动力学模拟。此软件基于Kohn-Sham框架的含时密度泛函理论，所有物理量于实空间规则网格下展开并执行实时模拟。软件已成功应用于多个物理系统的线性及非线性吸收谱、谐振谱、激光诱导瓦解。

###### 实空间方法的优点是整个模拟过程简单直观。首先，诸如密度或波函数等物理量易于实空间可视化。进一步，该方法在1-，2-或3-维系统及不同边界条件下相当易于进行数值计算。例如，我们不需要超胞也可研究有限体系、分子或团簇，只须简单地将距系统足够远处表面的波函数设置为零。同样地，有限体系、聚合物或体相材料在施加了合适的周期性边界条件后也能被加以研究。在实空间方法下只有一个收敛参数——网格步长，降低网格步长通常能有效提高计算结果质量。
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
    module load intel/2022 intelmpi/2022           # intel 环境加载

    ##############################################
    #               Run job                      #
    ##############################################
    export OMP_NUM_THREADS=1

    mpirun octopus_mpi >& log
    ```

***

#### 二、编译

1. 环境加载

    ```bash
    export LIBS_BLAS=basedir/lib/libblas.a
    export LIBS_LAPACK=basedir/lib/liblapack.a
    export LIBS_FFT=basedir/lib/libfftw3.a
    ```

2. 编写配置脚本：

    ```bash
    autoreconf -i

    ./configure CC="gcc -m64" CFLAGS="-O3 -march=native" FC="gfortran -m64" FCFLAGS="-O3 -ffree-line-length-none" --prefix=basedir  --with-gsl-prefix=basedir --with-libxc-prefix=basedir  --with-fftw-prefix=basedir

    make
    make install

    # 编译安装后将在 basedir/bin/octopus 生成
    ```

3. 完成。
