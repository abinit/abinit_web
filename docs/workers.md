## Buildbot worker matrix 

The current reference worker is "alps", with reference [builder](https://github.com/abinit/abinit_web/blob/final_burger/docs/builder.md) "alps_gnu_9.3_openmpi".


| Name   | CPU Type                | #Cores<br>(#THD) | Freq (GHz) | RAM (GB) | OS         | Compiler                                | MPI                           | Math                            | Misc               | Purchase         | S/N                   |
|--------|-------------------------|------------------|------------|----------|------------|-----------------------------------------|-------------------------------|--------------------------------|--------------------|------------------|-----------------------|
| abiref | Xeon<br>E5-2670v3       | 2×12 (48)        | 2.3        | 32       | CentOS 7.9 | gcc7.4/9.5<br>ifort-19.1.3             | openmpi-4.0.4<br>mpich-3.3/4 | OpenBLAS                       | ref<br>py36 ve     | 4/15 5Yrs        | HP DL360 G9<br>CZJ520082T |
| alps   | Xeon<br>Gold 6230       | 2×20 (80)        | 2.1        | 64       | CentOS 8.3 | gcc9.3/10.2<br>ifort-19.1.3<br>NAG-7.1<br>oneAPI | mpich-3.3<br>openmpi-4.0.5 | OpenBLAS<br>MKL 2020<br>ELPA | py36               | 6/20 3Yrs        | HP DL360 G10<br>CZ20190LT4 |
| atlas  | Xeon<br>E5-2623v4       | 2×4 (16)         | 2.6        | 64       | CentOS 7.9 | gcc7.4/9.2<br>ifort18/19.1             | openmpi-3.1.3<br>mpich-3.3.2 | OpenBLAS<br>MKL 2018           | py37 ve            | 2017             | Supermicro            |
| bob    | Xeon<br>E5-2603v3       | 2×6 (12)         | 1.6        | 16       | Fedora 39  | gcc13.2                                 |                               | Atlas 3.10                      | py3.12             | 4/16 4Yrs        | Dell R430 PowerEdge    |
| buda2  | Xeon<br>Silver 4110<br><span style="color:red;">2*K40+TitanV</span> | 2×8 (32) | 2.1        | 16       | CentOS 7.9 | gcc8.3/9.5<br>ifort17                 | openmpi-3<br>mpich-3.1       | Atlas 3.10<br>Magma1.5<br>GSL1.14 | cuda-11.2<br>py36 conda | 4/18 3Yrs        | TDH Supermicro        |
| eos    | AMD<br>EPYC 7643<br><span style="color:red;">2*A30</span> | 2×48 (192) | 2.3      | 256      | Ubuntu 22.04 | nvhpc23.9<br>oneAPI 2023<br>ifx<br>gnu 11.3 | openmpi-3<br>mpich-3.1   | Atlas 3.10<br>Magma1.5<br>GSL1.14 | cuda-12<br>py3.10 | 12/22 4Yrs       | Dell R7525 PowerEdge  |
| higgs  | Xeon<br>E5-2440         | 2×6 (24)         | 2.4        | 32       | Ubuntu 18.04LTS | gcc12.3<br>ifort19.0                 | openmpi-3.1<br>mpich-3.2     | MKL 2019                        | py37<br>conda      | 10/17 3Yrs       | HP DL360 G8<br>CZJ2511HHR |
| minimac| Apple M1 Ultra          | 16+4             | 3.4        | 64       | macOS 13.4.1 | gcc12                                | openmpi-3.1<br>mpich-3.2     | OpenBLAS                        | py37<br>conda      | 2/23 3Yrs        | Apple studio M1        |
| scope  | AMD<br>EPYC 7502        | 2×32 (128)       | 2.5        | 96       | Ubuntu 18.04LTS | gnu10.2/12.2/13.2                   | openmpi-4<br>mpich-3.3        | MKL 2020                        | py36               | 6/20 3Yrs        | HP DL385G10<br>CZJ520082V |
| ubu    | Xeon<br>E5-2670v3       | 2×12 (48)        | 2.3        | 32       | Ubuntu 16.04LTS | gcc9.3<br>ifort16                  | mpich-3.2                    | MKL 11.2                        | py37<br>conda      | 4/15 5Yrs        | HP DL360 G9<br>CZ20230JW5 |


#cores = # of Cores (hardware term that describes the number of independent central processing units)

#THD = # of Threads (software term for the basic ordered sequence of instructions that can be passed through or processed by a single CPU core) 
