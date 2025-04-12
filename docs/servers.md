## Buildbot testfarm server matrix 

The current reference [builder](https://github.com/abinit/abinit_web/blob/main/docs/builder.md) "eos_gnu_13.2_mpich", running on the server "eos".


| Name   | CPU Type<br>$${\color{red}GPU \space Type}$$    | #Cores<br>(#THD)  | RAM (GB) | OS         | Compiler                                | MPI                           | Math                            | Misc               | Purchase         | S/N                   |
|--------|-------------------------|------------------|----------|------------|-----------------------------------------|-------------------------------|--------------------------------|--------------------|------------------|-----------------------|
| alps   |  Xeon 6230  | 2×20 (80)        | 64       | Rocky-9 | gcc9.5<br>NAG-7.2<br>oneAPI 2024<br>oneAPI 2025 | mpich-3.3<br>openmpi-4.0.5 | OpenBLAS<br>MKL 2020<br>ELPA | py39               | 6/20 3Yrs        | HP DL360 G10<br>CZ20190LT4 |
| bob    |  Xeon E5-2603       | 2×6 (12)         | 16       | Fedora-39  | gcc13.2                                 |                               | Atlas 3.10                      | py3.12             | 4/16 4Yrs        | Dell R430 PowerEdge    |
| buda2  |  Xeon 4110<br>$${\color{red}2*K40+TitanV}$$  | 2×8 (32) | 16       | CentOS-7.9 | gcc8.3/9.5<br>ifort17                 | openmpi-3<br>mpich-3.1       | Atlas 3.10<br>Magma1.5<br>GSL1.14 | cuda-11.2<br>py36 conda | 4/18 3Yrs        | TDH Supermicro        |
| eos    |  AMD 7643<br>$${\color{red}2*A30}$$  | 2×48 (192) | 256      | Ubuntu-22.04 | nvhpc23.9<br>oneAPI 2023<br>gnu 11.3 | openmpi-3<br>mpich-3.1   | Atlas 3.10<br>Magma1.5<br>GSL1.14 | cuda-12<br>py3.10 | 12/22 4Yrs       | Dell R7525 PowerEdge  |
| higgs  |  Xeon E5-2440         | 2×6 (24)         | 32       | Ubuntu-18.04 | gcc12.3                | openmpi-3.1<br>mpich-3.2     | MKL 2019                        | py37<br>conda      | 10/17 3Yrs       | HP DL360 G8<br>CZJ2511HHR |
| minimac|  Apple M1 Ultra          | 16+4             | 64       | macOS-15.1.1 | gcc12                                | openmpi-3.1<br>mpich-3.2     | OpenBLAS                        | py37<br>conda      | 2/23 3Yrs        | Apple studio M1        |
| scope  |  AMD 7502        | 2×32 (128)       | 96       | Ubuntu-18.04 | gnu10.2/12.2<br>gnu13.2                   | openmpi-4<br>mpich-3.3        | MKL 2020                        | py36               | 6/20 3Yrs        | HP DL385G10<br>CZJ520082V |
| ubu    |  Xeon E5-2670       | 2×12 (48)        | 32       | Ubuntu-16.04 | gcc9.3<br>ifort16                  | mpich-3.2                    | MKL 11.2                        | py37<br>conda      | 4/15 5Yrs        | HP DL360 G9<br>CZ20230JW5 |


#cores = # of Cores (hardware term that describes the number of independent central processing units)

#THD = # of Threads (software term for the basic ordered sequence of instructions that can be passed through or processed by a single CPU core) 
