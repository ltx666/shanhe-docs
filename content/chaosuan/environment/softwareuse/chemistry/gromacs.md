## GROMACS 是一种分子动力学应用程序
###### 可以模拟具有数百至数百万个粒子的系统的牛顿运动方程。GROMACS旨在模拟具有许多复杂键合相互作用的生化分子，例如蛋白质，脂质和核酸。
***
#### 一、脚本模板
1. Slurm
    ```bash
    #!/bin/bash
    #SBATCH --nodes=1                   # 节点数量
    #SBATCH --ntasks-per-node=56        # 每个节点核心数量
    #SBATCH --ntasks=56                 # 总核心数
    #SBATCH --partition=g1_share        # 队列分区且必须指定正确分区
    #SBATCH --job-name=gromacs    	    # 作业名称
    #SBATCH --output=gromacs.%j.out     # 正常日志输出 (%j 参数值为 jobId)
    #SBATCH --error=gromacs.%j.err      # 错误日志输出 (%j 参数值为 jobId)

    ##############################################
    #          Software Envrironment             #
    ##############################################
    unset I_MPI_PMI_LIBRARY                        # 取消默认mpi库，使用intel自带
    export I_MPI_JOB_RESPECT_PROCESS_PLACEMENT=0   # intel 多节点作业所需修改参数 
    module load intel/2022 intelmpi/2022           # intel 环境加载
    moudle load gromacs                            # 软件加载(参考module使用方法，选择具体版本)

    ##############################################
    #               Run job                      #
    ##############################################

    pdb2gmx -ignh -ff gromos43a1 -f 1OMB.pdb -o fws.gro -p fws.top -water none
    editconf -f fws.gro -d 0.7
    editconf -f out.gro -o fws_ctr.gro -center 2.0715 1.6745 1.914
    grompp -f em.mdp -c fws_ctr.gro -p fws.top -o fws_em.tpr
    srun gmx_mpi mdrun -s fws_em.tpr -o fws_em.trr -c fws_ctr.gro -g em.log -e em.edr
    # srun gmx_mpi mdrun -v -deffnm step7_production
    ```

***

#### 二、编译
1.  选择版本并下载解压源码
    ```bash
    $ https://manual.gromacs.org/documentation/
    ```
2. 登录节点禁止运行作业和并行编译，请申请计算节点资源用来编译，并在编译结束后退出：
    ```bash
    $ srun -p g1_share -n 56 --pty /bin/bash
 
    # -p 参数指定了队列，请以实际使用为准
    ```
3. 环境加载

    ```bash
    module load gcc/12.1.0 cmake/3.21.1 openmpi/4.1.3-gcc12.1.0
    加载gcc环境
    export CC=`which gcc`
    export CXX=`which g++`
    export FC=`which gfortran`
    ```

4. FFTW安装

    ```bash
    fftw下载路径：https://fftw.org/pub/fftw/
    tar -xf fftw-3.3.9.tar.gz
    cd fftw-3.3.9/
    ./configure --prefix=/es01/home/xiehx/fftw --enable-mpi --enable-static --enable-shared CC=gcc
    make -j 20
    make install
    ```

5. openblas安装

    ```bash
    openblas下载链接：https://sourceforge.net/projects/openblas/
    tar -xf Openblas-0.3.20.tar.gz
    cd Openblas-0.3.20
    make
    make PREFIX=/es01/home/xiehx/OpenBLAS install
    ```

6. lapack安装

    ```
    lapack下载链接：http://www.netlib.org/lapack
    tar -xf lapack-3.10.1.tar.gz
    cd lapack-3.10.1
    cp make.inc.example make.inc
    make
    cd LAPACKE
    make
    cd ../ 
    mkdir lib
    mv *.a lib 
    ```

7. gromacs安装

    ```
    gromacs下载链接：https://manual.gromacs.org/current/download.html#
    tar xf gromacs-2022.tar.gz
    cd gromacs-2022
    mkdir build ; cd build
    PREFIX=/es01/home/xiehx/gromacs
    LDFLAGS="-lgfortran" CC=mpicc CXX=mpicxx cmake -DCMAKE_INSTALL_PREFIX=$PREFIX -DBUILD_SHARED_LIBS=on -DBUILD_TESTING=on -DREGRESSIONTEST_DOWNLOAD=off -DGMX_BUILD_OWN_FFTW=off -DGMX_DOUBLE=on -DGMX_EXTERNAL_BLAS=on -DGMX_EXTERNAL_LAPACK=on -DGMX_FFT_LIBRARY=fftw3 -DGMX_BLAS_USER=/es01/home/xiehx/OpenBLAS/lib/libopenblas.a -DGMX_LAPACK_USER=/es01/home/xiehx/lapack/lapack-3.10.1/lib/liblapack.a -DFFTW_LIBRARY=/es01/home/xiehx/fftw/lib/libfftw3.so -DFFTW_INCLUDE_DIR=/es01/home/xiehx/fftw/include -DGMX_GPU=off -DGMX_MPI=on -DGMX_OPENMP=on -DGMX_X11=off ../
    ```

    ###### 附ERROR：lapack64位链接错误处理方法：

    ```bash
    # 使用-fPIC重新编译lapack本身.所以在你的make.inc中更改为以下内容:
    FORTRAN  = gfortran -m64 -fPIC
    OPTS     = -O2 -m64 -fPIC
    DRVOPTS  = $(OPTS)
    NOOPT    = -O0 -m64 -fPIC
    LOADER   = gfortran -m64 -fPIC
    ```

