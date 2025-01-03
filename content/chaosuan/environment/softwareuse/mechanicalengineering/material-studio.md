---
title: "MS"
description: Test description
draft: false
enableToc: false
keyword: test
weight: 13
---

## MS 材料模拟和建模

###### 这个软件被许多大学、研究中心以及高科技公司使用，以研究各种各样的材料，比如高分子、碳纳米管、催化剂、金属、陶瓷等等

###### 请自行购买MS license许可，下载和安装。如需协助安装或使用，请联系我们，附上课题组拥有MS license的证明。

***
#### 一、脚本模板
1. Slurm

    ```bash
    #!/bin/bash
    #SBATCH --nodes=1                   # 节点数量
    #SBATCH --ntasks-per-node=56        # 每个节点核心数量
    #SBATCH --ntasks=56                 # 总核心数
    #SBATCH --partition=g1_share        # 队列分区且必须指定正确分区
    #SBATCH --job-name=ms       	    # 作业名称
    #SBATCH --output=ms.%j.out          # 正常日志输出 (%j 参数值为 jobId)
    #SBATCH --error=ms.%j.err           # 错误日志输出 (%j 参数值为 jobId)

    ##############################################
    #          Software Envrironment             #
    ##############################################
    unset I_MPI_PMI_LIBRARY                        # 取消默认mpi库，使用intel自带
    export I_MPI_JOB_RESPECT_PROCESS_PLACEMENT=0   # intel 多节点作业所需修改参数 
    # module load intel/2022 intelmpi/2022           # intel 环境加载

    ##############################################
    #               Run job                      #
    ##############################################
    RunCASTEP.sh -np 56 Edit_job_File_name
    ```

***

#### 二、安装

1. 解压缩文件 Materials Studio60.tar 得到 Materials Studio60 文件目
录，进入 Materials Studio60 文件目录开始安装。

```bash
tar -xvf MaterialsStudio60.tar
cd MaterialsStudio60

./install
# (均默认安装即可)

# 注意1：若安装开始时，遇到“Bundled JRE is not binary compatible with host OS/Arch or it is corrupt. Testing bundled JRE failed.”的错误提示，请安装一下 glibc 的 32 位库文件。

# 注意2: 超算中心禁止用户使用客户端/服务器端模式使用 MaterialsStudio 软件，因此软件安装完成后，请将 gateway 进程停用：
Accelrys/MaterialsStudio6.0/etc/Gateway/gwstop

# 导入软件许可：
Accelrys/LicensePack/linux/bin/lp_install soft/msi.lic
```
