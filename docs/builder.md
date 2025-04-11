## Buildbot builder matrix 

### The reference builder 

The current reference builder is "eos_gnu_13.2_mpich" (EOF) running on the "eos" testfarm [server](https://github.com/abinit/abinit_web/blob/main/docs/servers.md).
In order to understand the rationale behind the set of the different builders let's first detail this reference builder EOF.

eos_gnu_13.2_mpich (EOF) is characterized by the following elements :
  * The compiler is gcc 13.2 with<br>"-O2 -Wno-maybe-uninitialized -ffpe-trap=invalid,zero,overflow -fcheck=all,no-pointer -fallow-argument-mismatch<br>-g -Wall -fbacktrace -lcurl" flags.
  * The MPI is MPICH version 4.2.2.
  * The compilation is done with "make -j 8", with the external libraries : NetCDF-C(-Fortran), HDF5, LibXC, OpenBLAS, Wannier90, BigDFT and PSML, build with MPI and with MPI_IO .
  * GW is compiled with double precision
  * The tests are executed with "./runtests.py" (and the list of tests is coherent with the fallbacks and flags)

This reference builder is not doing everything :
  * The buildsys, abirules,  seq, tparal, gpu tests are not executed by EOF
  * OpenMP, and memory profiling is not activated for EOF
  * Several procedures are not tested by EOF, like distchck.
  * GW is not tested in single precision
  * BigDFT tests are not executed (for the time being)

### The other builders 

Each one of the other builders departs from the reference, in order to test the portability of the build system and automatic tests.
The characteristics of this departure are indicated in the last column of the table.

Thus some reference files cannot be generated on eos. 
For this purpose, auxiliary reference builders are provided :
  * eos_gnu_13.2_serial (EOZ) (for the seq tests)
  * scope_gnu_10.2_paral (SCO) (for the tparal tests, that need more than 10 procs)

Also, some bots provide unique services :
  * bob_gnu_13.2_openmp and ubu_intel_16.0_openmp (OpenMP)
  * scope_gnu_12.2_mpich (enable_memory_profiling=yes, detect memory leak)
  * alps_gnu_9.5_sdebug (for buildsys, abirules, distchk [checking the production of the .tar.gz package] and html link checker)...
  * scope_gnu_13.2_dep check dependency
  * higgs_gnu_12.3_cov and ubu_intel_16.0_mpich test the BigDFT library.

### Matrix of builders 

| Slave  | #Nightly | Builder                     | Nightly | Compiler     | MPI           | Linear Algebra     | Libs Tested     | Departure from Ref                          |
|--------|---------|------------------------------|---------|--------------|---------------|--------------------|----------------|---------------------------------------------|
| alps   | 2       | alps_gnu_9.5_sdebug          | yes      | gnu-9.5      | mpich-4.2.3   | OpenBLAS<br>fftw3 | ABPW | -fno-frontend-optimize<br>-ffpe-trap=i,z,o |
|        |         | alps_nag_7.2_serial          | yes      | nag-7.2      |               | netlib_3.10.0      | A              | enable-netcdf-default                       |
|        |         | alps_nag_7.2_openmpi         | no       | nag-7.2      | openmpi-4.1.2 | netlib_3.10.0      | A              | enable-netcdf-default                       |
|        |         | alps_intel_2024_oneAPI       | no       | intel<br>oneAPI_2024.2 | intel mpi  | mkl 2024.2 | APW | scalapack ifort 2021.13 |
|        |         | alps_intel_2025_oneAPI       | no       | intel<br>oneAPI_2025.0 | intel mpi  | mkl 2024.2 | APW | scalapack ifx  |
| bob    | 1       | bob_gnu_13.2_openmp          | yes      | gnu-13.2     |               | atlas-3.10         | P              | Fedora39 packages                           |
| buda2  | 1       | buda2_gnu_8.5_cuda           | yes      | gnu-8.5      | openmpi-4.1.3 | mkl-2019.0.1<br>cuda 11.2 | | enable_gpu |
| eos    | 3       | eos_nvhpc_23.9_elpa          | yes      | nvhpc 23.9   | openmpi-4.1.6 | mkl-2023 elpa-2022                   |                | cuda-12                     |
|        |         | eos_nvhpc_24.9_openmpi       | no       | nvhpc 24.9   | openmpi-4.1.6 | mkl-2023            |   |       |    cuda 12.2                                       |
|        |         | eos_gnu_13.2_mpich           | reference  | gnu-13.2   | mpich-4.2.3   | mkl 2023.2 | ABPW | -fcheck=all,no-pointer<br>-ffpe-trap=i,z,o<br>-fallow-argument-mismatch |
|        |         | eos_gnu_13.2_serial          | reference  | gnu-13.2   |               | mkl 2023.2 | ABPW | -fcheck=all,no-pointer<br>-ffpe-trap=i,z,o<br>-fallow-argument-mismatch |
| higgs  | 1       | higgs_gnu_12.3_cov           | yes      | gnu-12.3     | mpich-4.1.2   | mkl 2019           | ABPW           | coverage analysis<br>enable-netcdf-default |
| scope  | 3       | scope_gnu_10.2_paral         | ref for tparal | gnu-10.2 | mpich-3.2 | OpenBLAS | BPW | mpirun -np 2 if max_nprocs allows it<br>enable-netcdf-default GW_SP |
|        |         | scope_gnu_13.2_dep           | yes      | gnu-13.2     | mpich-4.1.2   | OpenBLAS           | PW             | check dependency<br>enable-netcdf-default |
|        |         | scope_gnu_12.2_mpich         | yes      | gnu-12.2     | mpich-4.0.3   | OpenBLAS           | PW             | enable_memory_profiling                    |
|        |         | scope_gnu_12.2_abipy         | yes      | gnu-12.2     | mpich-4.0.3   | OpenBLAS           | PW             | check abipy                                |
|        |         | scope_gnu_10.2_s64           | odonly   | gnu-10.2     | mpich-3.2     | OpenBLAS           | PW             | tutoparal with np=64                       |
| ubu    |  1      | ubu_intel_16.0_openmp        | yes      | intel-16.0   |               | mkl 11.3           | A              | OpenMP / dfti                              |

*Caption for external fallbacks : A= AtomPAW, B= BigDFT, P= PSML+XMLF90, W= Wannier90

*Mandatory fallbacks : linalg, netCDF-C/netCDF-Fortran with HDF5 support and libXC 
