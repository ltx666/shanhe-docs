---
title: "ROMs"
description: Test description
draft: false
enableToc: false
keyword: test
weight: 13
---

## ROMs

###### ROMS是海洋研究机构常用的区域和大洋环流模式，已经在CO-OPS开始得到大量使用，并且将其列为业务化海洋模式之一。ROMS模式采用F90/F95编写，代码结构依据网格特点编写，支持共享存储和消息传递两种多处理器体系架构，其对集群性能要求较高，具有计算密集、IO读写量较大、网络延迟带宽要求高的特点。模式依据问题规模可达到较好扩展性，规模较小时可加速到将近100core，较大时可达到400-1000core。同时，该应用也可跟气象海洋上其他模式组成耦合模式，或者作为其他气象海洋应用的驱动程序，在国内海洋机构中的应用也比较多。

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

    mpirun ./oceanM ocean_upwelling.in
    ```

***

#### 二、编译

```bash
tar -xf roms.tar.gz -C /test/roms/
cp ./roms/ROMS/Bin/build_roms.sh roms_intel/
cp ./roms/ROMS/Include/upwelling.h roms_intel/
cp ./roms/ROMS/External/roms_upwelling.in roms_intel/
cp ./roms/ROMS/External/varinfo.dat roms_intel/            				# 高版本可能是.yaml文件
```

```bash
vim build_roms.sh
# export which_MPI=openmpi                                              # 注释第四十七行
export  MY_ROOT_DIR=/test/roms                                          # 111行更改
export  MY_ROMS_SRC=${MY_ROOT_DIR}/roms                                 #124行更改
# export which_MPI=openmpi # 168行
export       USE_NETCDF4=on            # compile with NetCDF-4 library  #178-180行取消注释
export          USE_HDF5=on            # compile with HDF5 library
export   USE_PARALLEL_IO=on            # Parallel I/O with NetCDF-4/HDF5

# 2022版本增加几条更改
export          USE_HDF5=on            # compile with HDF5 library      # 取消206行注释
#export USE_MY_LIBS=no            # use system default library paths    # 注释掉系统默认路径，解除自己路径注释
export USE_MY_LIBS=yes           # use my customized library paths
```

```bash
vim roms_upwelling.in
VARNAME = /test/roms/roms_intel/varinfo.yaml 						    # 更改76行,根据实际版本更改可能是.yaml或者.dat文件
NtileI == 1                               ! I-direction partition       # 109-120行根据实际所需修改
NtileJ == 1                               ! J-direction partition
```

```bash
cd  /test/roms/roms/Compilers
cp  my_build_paths.sh my_build_paths_beifen.sh 						    # 更改配置文件做好备份
vim my_build_paths.sh
export               MPI_ROOT="# which mpi"                             # 51行更改mpi实际路径
# 文档末尾添加
export HDF5=/test/roms/roms_lib
export HDF5_INCDIR=${HDF5}/include
export HDF5_LIBDIR=${HDF5}/lib
export PATH=${HDF5}/bin:$PATH
export LD_LIBRARY_PATH=${HDF5}/lib:$LD_LIBRARY_PATH
export NETCDF=/test/roms/roms_lib
export NC_CONFIG=${NETCDF}/bin/nc-config
export NETCDF_LIBDIR=${NETCDF}/lib
export NETCDF_INCDIR=${NETCDF}/include
export PATH=${NETCDF}/bin:$PATH
export LD_LIBRARY_PATH=${NETCDF}/lib:$LD_LIBRARY_PATH
```

```bash
vim Linux-ifort.mk 														# 同目录
ifdef USE_NETCDF4                                                       # 198行
        NF_CONFIG ?= /test/roms/roms_lib/bin/nf-config
       NETCDF_INCDIR ?= /test/roms/roms_lib/include
       	 NETCDF_LIBS ?= /test/roms/roms_lib/lib
             LIBS += -lnetcdf -lnetcdff -lhdf5l -lz
           INCDIR += $(NETCDF_INCDIR) $(INCDIR)
ifdef USE_HDF5
      HDF5_INCDIR ?= /test/roms/roms_lib/include                        # 211行
      HDF5_LIBDIR ?= /test/roms/roms_lib/lib
        HDF5_LIBS ?= -lhdf5_fortran -lhdf5hl_fortran -lhdf5 -lz
             LIBS += -L$(HDF5_LIBDIR) $(HDF5_LIBS)
           INCDIR += $(HDF5_INCDIR)
ifdef USE_MPI                                                           #228行
         CPPFLAGS += -DMPI
 ifdef USE_MPIF90
  ifeq ($(which_MPI), intel)
               FC := mpiifort
  else
               FC := mpiifort                                           #更改为mpiifort
  endif
 else
             LIBS += -lfmpi -lmpi
 endif
endif
# 编辑成功后执行安装脚本
```

