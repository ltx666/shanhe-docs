---
title: "LAMMPS"
description: Test description
draft: false
enableToc: false
keyword: test
weight: 9
---
## LAMMPS 是由桑迪亚国家实验室开发的一套分子动力学模拟的开源程序包

###### LAMMPS提供了元素周期表中原子对应的经验势函数，可进行与实际体系对应的分子动力学模拟，例如计算金属铝的晶格常数，或水的密度，得到与实验吻合的结果。另一方面也提供了多种模型势函数，可用于进行粗粒化模拟，例如模拟基于粒子-弹簧模型的高分子链的性质。 LAMMPS作为实用程序包，采取了很多成熟的优化算法。例如在短程相互作用的计算中运用韦尔莱表和元胞列表优化临近粒子的扫描。
***
#### 一、脚本模板
1. Slurm
   ```bash
   #!/bin/bash
   #SBATCH --nodes=1                   # 节点数量
   #SBATCH --ntasks-per-node=56   	    # 每个节点核心数量
   #SBATCH --ntasks=56                 # 总核心数
   #SBATCH --partition=g1_share   	    # 队列分区且必须指定正确分区
   #SBATCH --job-name=lammps    	      # 作业名称
   #SBATCH --output=lammps.%j.out       # 正常日志输出 (%j 参数值为 jobId)
   #SBATCH --error=lammps.%j.err        # 错误日志输出 (%j 参数值为 jobId)

   ##############################################
   #          Software Envrironment             #
   ##############################################
   unset I_MPI_PMI_LIBRARY                        # 取消默认mpi库，使用intel自带
   export I_MPI_JOB_RESPECT_PROCESS_PLACEMENT=0   # intel 多节点作业所需修改参数 
   module load intel/2022 intelmpi/2022           # intel 环境加载
   module load lammps                             # 软件加载(参考module使用方法，选择具体版本)

   ##############################################
   #               Run job                      #
   ##############################################
   ulimit -s unlimited
   ulimit -l unlimited
   mpirun lmp_mpi -i in.lj.txt                    # 将in.lj.txt 修改为实际输入
   ```

***

#### 二、编译
1. 官网下载 lammps最新的稳定版：
   ```
   $ https://lammps.sandia.gov/tars/lammps-stable.tar.gz
   ```
2. 登录节点禁止运行作业和并行编译，请申请计算节点资源用来编译，并在编译结束后退出：
   ```bash
   $ srun -p g1_share -n 56 --pty /bin/bash

   # -p 参数指定了队列，请以实际使用为准
   ```
3. 加载 Intel 模块：
   ```
   $ module load intel/2022 intelmpi/2022           # intel 环境加载
   ```

4. **CPU**编译 (以额外安装 MANYBODY 和 Intel 加速包为例)
   ```
   $ tar xvf lammps-stable.tar.gz
   $ cd lammps-XXXXXX
   $ cd src
   $ make                           #查看编译选项
   $ make package                   #查看包
   $ make yes-intel                 #"make yes-"后面接需要安装的 package 名字
   $ make yes-manybody
   $ make ps                        #查看计划安装的包列表
   $ make -j intel_cpu_intelmpi     #开始编译
   ```

5. **GPU**编译
   ```bash
   ```

#### 三、kokk包

```bash
##Kokkos 是一个模板化C++库，它提供了抽象以允许 应用程序内核的单个实现（例如配对样式） 在不同类型的硬件上高效运行，例如 GPU、英特尔 至强Phis，或众核CPU。Kokkos 将C++内核映射到 不同的后端语言，如CUDA，OpenMP或Pthreads。这 Kokkos 库还提供了要调整的数据抽象（在编译 time）数据结构（如2D和3D数组）的内存布局 优化不同硬件的性能。
#1、修改 kokkos 里面对gpu cpu 的架构参数 下文为 gpu A100  cpu Intel(R) Xeon(R) Gold 6258R CPU  具体参照 https://docs.lammps.org/Build_extras.html#kokkos
vim src/MAKE/OPTIONS/Makefile.kokkos_cuda_mpi

KOKKOS_ARCH=SKX,Ampere80

make yes-manybody yes-molecule yes-replica yes-kspace yes-asphere yes-rigid yes-openmp yes-reaxff yes-ml-snap yes-kokkos

make kokkos_cuda_mpi –j 16
###运行命令 
mpirun -np 2 --allow-run-as-root --mca btl '^openib' /data/software/hpctest/lammps-15Sep2022/src/lmp_kokkos_cuda_mpi -k on g 2 -sf kk -in lammps-23Jun2022/examples/flow/in.flow.couette

#####注意：在写kokko 要求的填写 cpu gpu 架构那lammps 版本不一致的话，大小写要求也不一样
##参考连接 https://docs.lammps.org/Build_extras.html#kokkos
```

#### 四、nnp 包

```bash 
1、加载 Intel 及intelmpi 环境
2、下载 n2p2 包 tar xf 解压 后 进入 src 	目录
3、执行 make lammps-nnp 查看提示下载 lammps 版本
4、lammps 官网下载 提示版本lammps 
5、解压 lammps 
6、vim修改 n2p2源码包下 src/interface/makefile 文件
7、删除 if.... wget ..mv lammps..lammps-nnp 这几行，并记下最后mv 给lammps 源码包改的名字 (lammps-nnp),保存后退出
8、将解压后的lammps 包 mv 到 n2p2源码包下 src/interface/ 并改为 src/interface/makefile文件提示的lammps-nnp
9、cd ../
10、执行 make lammps-nnp
11、lmp_mpi 二进制包生成于n2p2源码包下的bin目录

```
