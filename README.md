# tSDRG
Tree tensor strong disorder renormalization group.

Installation
-------
One may choose from one of these methods below

#### I. Docker container
One may build the Docker image with
```
docker build --no-cache --force-rm -t tsdrg .
```
and launch the container interactively
```
docker run --rm -it tsdrg
```
See the documentation [here](https://docs.docker.com/engine/reference/run/) 
if you want to run the container in background,
or to mount the system volume into container.

> **_Note:_** The size of this image is about 22.8GB, mainly because of Intel oneAPI.

#### II. Singularity container
Currently, to keep the consistency, we use Docker image to generate Singularity container. 

To build the singularity image file (`.sif`),
it is required to have a Dockerhub account for hosting the Docker image.
One could therefore build `.sif` extension with
```
singularity build tsdrg.sif docker://<username>/<imagename>
```
One can then interact with the image in [several ways](https://sylabs.io/guides/3.0/user-guide/quick_start.html#interact-with-images).
For instance,
```
singularity shell tsdrg.sif
```
See [here](https://sylabs.io/guides/3.0/user-guide/singularity_and_docker.html) for more information about working Docker with Singularity.

> **_Note_**: By default, 
> Singularity bind mounts /home/$USER, /tmp, and $PWD automatically into your container at runtime.
> This is different from Docker.

#### III. Build from scratch
  
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
For instance, 
```
- data/
    - PBC/
        - Jdis010/
            - Dim010/
                - L32_P10_m8_1/
                    - J_list.csv
                    - L32_P10_m8_1_corr2.csv
                    - ZL.csv
                    - w_loc.csv
                    - L32_P10_m8_1_corr1.csv
                    - L32_P10_m8_1_string.csv
                    - energy.csv  
```

References
-------
* [Yu-Ping Lin, Ying-Jer Kao, Pochung Chen, and Yu-Cheng Lin,
  Griffiths singularities in the random quantum Ising antiferromagnet: A tree tensor network renormalization group study,
  Phys. Rev. B 96, 064427 (2017)](
  https://journals.aps.org/prb/abstract/10.1103/PhysRevB.96.064427)
* [Kedar Damle, and David A. Huse,
  Permutation-Symmetric Multicritical Points in Random Antiferromagnetic Spin Chains,
  Phys. Rev. Lett. 89, 277203 (2002)](
  https://journals.aps.org/prl/abstract/10.1103/PhysRevLett.89.277203)
