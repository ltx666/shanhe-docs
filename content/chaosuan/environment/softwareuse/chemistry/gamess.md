---
title: "GAMESS"
description: Test description
draft: false
enableToc: false
keyword: test
weight: 7
---
## GAMESS

###### General Atomic and Molecular Electronic Structure System (GAMESS) 是一款分子层面的从头算量子化学程序。GAMESS可用于计算RHF、ROHF、UHF、GVB乃至MCSCF的SCF波函数。其中针对这些SCF波函数的关联修正包括组态相互作用、二阶微扰、耦合团簇方法和密度泛函理论近似。软件可通过CI、EOM及TD-DFT（含时密度泛函）计算激发态。同时，原子核梯度方法在结构自动优化、过渡态搜索或反应路径追踪功能实现上也是可用的。通过能量hessian计算，软件可以预测振动频率、IR或拉曼强度。借助于离散有效片段势（discrete Effective Fragment potentials）或极化连续体模型等连续体模型，软件能够模拟溶剂效应。软件还能从事多种相对论计算，包括有限阶的二组分标量相对论修正（可考虑多种自旋轨道耦合方式）。值得一提的是，软件的分子轨道方法将计算划分为许多小片段，使得上述这些复杂处理方法可用在非常庞大的体系当中。此外，原子核波函数也是可以计算的，包括利用VSCF，或利用NEO代码对原子核轨道进行精细处理。

###### GAMESS可以计算许多分子性质，从简单的偶极子矩到依赖于频率的超极化率都属于GAMESS可研究的范围。GAMESS内存储了大量基组及有效核势、模型核势，其研究对象涵盖了几乎整个元素周期表。
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

    ./runall  00  >&  runall.log
    ```

***

#### 二、编译

1. 环境加载

    ```bash
    module load intel/2022 intelmpi/2022
    ```

2. 解压Gamess：

```bash
tar -xzvf gamess-current.tar.gz
```

3. 生成配置文件，

```bash
cd gamess

./config
```

机器名，一般输入linux64

回车（使用默认路径）

回车（使用默认路径）

回车（gamess默认的版本号00）

gfortran

4.8

回车

none(选择使用数学库，我这里为了避免麻烦，选择了none)

回车

sockets(选择sockets或者mpi)

no(不使用LIBCCHEM)

生成install.info和Makefile文件

4. 编译ddi

```bash
cd ddi

./compddi

mv ddikick.x /home/quant/games/ddikick.x
```

5. 编译compall

gamess文件夹下，./compall(时间很长)，完成后在gamess文件夹下产生gamess.00.x程序。

```bash
./lked gamess 00 2>&1 | tee lked.log
```

lked.log文件会显示过程，有The linking of GAMESS to binary gamess.00.x wassuccessful.字样说明已经连接成功。
