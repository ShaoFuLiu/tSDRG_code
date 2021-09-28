# tSDRG
Tree tensor strong disorder renormalization group.

Installation
-------

#### Docker

```
docker build --no-cache --force-rm -t tsdrg .
```
  
#### Build from scratch
  
1. Install Intel oneAPI
    
    Intel [Base toolkit](https://software.intel.com/content/www/us/en/develop/tools/oneapi/base-toolkit.html)
    and [HPC toolkit](https://software.intel.com/content/www/us/en/develop/tools/oneapi/hpc-toolkit.html) 
    are required for compiling Uni10 later.

2. Install Uni10
   
    Current dependency of this module uses Uni10 v.2.0.0,
    which has been attached in this repo,
    and one can unzip them by using
    ```
    tar -xzvf uni10-2.0.0.tar.gz
    ```
   
    One can therefore compile the source code by following the instruction [here](https://gitlab.com/uni10/uni10).
    It is recommended to use MKL and Intel compiler for compiling.

3. Set up the environment variables
    
    ```
    export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/uni10/lib/
    export C_INCLUDE_PATH=$C_INCLUDE_PATH:/usr/local/uni10/include/
    export CPLUS_INCLUDE_PATH=$CPLUS_INCLUDE_PATH:/usr/local/uni10/include/
    ```
    
Running a script
-------
The main script is put in `Main/`.
One can compile it with `Main/makefile` by targeting to the source code,
such as
```
make code=Heisenberg_spin1.cpp
```
Executable will be named as `job.exe`,
one could pass the parameters as
```
./job.exe {L} {chi} {Pdis} {Jdis} {Dim} {BC} {seed1} {seed2}
```
where
* **L**: system size
* **chi**: bond dimension, number of kept states in the triangular tensor (projector)
* **Pdis**: the distribution of random variable 
* **Jdis**: disorder strength of the coupling J
* **Dim**: Dimerization constant
* **BC**: boundary condition, `PBC` or `OBC`.
* **seed1**: random seed number in order to repeat data
* **seed2**: random seed number in order to repeat data

Analyzing the data
-------

Data will be saved into `Main/data` in `.csv` extension,
and with hierarchical folder structure to the input parameters.