## Buildbot builder matrix 

### The reference builder 

The current reference [workers](https://github.com/abinit/abinit_web/blob/final_burger/docs/workers.md) is "alps", with reference builder "alps_gnu_9.3_openmpi (LPR)".\\  In order to understand the rationale behind the set of the different builders let's first detail this reference builder LPR.

LPR "alps_gnu_9.3_openmpi" is characterized by the following elements :
  *  The compiler is gcc 9.3 \\ with "-O2 -g -Wall -Wno-maybe-uninitialized -ffpe-trap=invalid,zero,overflow -fbacktrace -pedantic -fcheck=all" flags.
  *  The MPI is Open MPI version 4.0.4.
  *  The compilation is done with "make -j 8", with the external libraries : NetCDF-C(-Fortran), HDF5, LibXC, OpenBLAS, Wannier90, BigDFT and PSML, build with MPI and with MPI_IO .
  * GW is compiled with double precision
  * The tests are executed with "./runtests.py" (and the list of tests is coherent with the fallbacks and flags)



This reference builder is not doing everything :
  * The buildsys, abirules, robodoc-parent-abiauty, seq, tparal, gpu tests are not executed by LPR
  * OpenMP, and memory profiling is not activated for LPR
  * Several procedures are not tested by LPR, like distchck.
  * GW is not tested in single precision
  * BigDFT tests are not executed (for the time being)

### The other builders 

Each one of the other builders departs from the reference, in order to test the portability of the build system and automatic tests.\\
The characteristics of this departure are indicated in the last column of the table.

Thus some reference files cannot be generated on abiref. For this purpose, auxiliary reference builders are provided :
  * alps_gnu_9.3_serial (for the seq tests)
  * scope_gnu_10.2_paral (for the tparal tests, that need more than 10 procs)

Also, some bots provide unique services :
  * bob_gnu_13.2_openmp and ubu_intel_16.0_openmp (OpenMP)
  * scope_gnu_12.2_mpich (enable_memory_profiling=yes, detect memory leak)
  * abiref_gnu_9.5_sdebug (for buildsys, abirules, robodoc, distchk [checking the production of the .tar.gz package], html link checker, and robodoc-html tar.gz package)...
  * scope_gnu_13.2_dep check dependency
  * higgs_gnu_12.3_cov and ubu_intel_16.0_mpich test the BigDFT library.

### Matrix of builders 

| Slave  | #Nightly Builds | Builder                     | Nightly? | Compiler     | MPI           | Linear Algebra     | Libs Tested     | Departure from Ref                           |
|--------|----------------|------------------------------|----------|--------------|---------------|--------------------|----------------|---------------------------------------------|
| abiref | 2              | abiref_gnu_9.5_sdebug        | yes      | gnu-9.5      | mpich-4.0.2   | OpenBLAS_0.3.9     |                | many services<br>scalapack                  |
|        |                | abiref_intel_19.1_mpich      | yes      | intel-19.1.3 | mpich-3.3.2   | mkl 2020           | APW            | scalapack enabled                           |
| alps   | 4              | alps_gnu_9.3_openmpi         | reference| gnu-9.3      | openmpi-4.0.4 | OpenBLAS_0.3.10<br>fftw3 | ABPW | -fno-frontend-optimize<br>-ffpe-trap=i,z,o |
|        |                | alps_gnu_9.3_serial          | reference| gnu-9.3      |               | OpenBLAS_0.3.10<br>fftw3 | APW | fcheck=all<br>-ffpe-trap=i,z,o             |
|        |                | alps_intel_21.4_elpa         | yes      | intel<br>oneAPI_2021.4 | intel mpi  | mkl 2021.4 | APW | scalapack<br>elpa 2020.11 |
|        |                | alps_nag_7.1_openmpi         | yes      | nag-7.1      | openmpi-4.1.2 | netlib_3.10.0      | A              | enable-netcdf-default                       |
| atlas  | 2              | atlas_intel_18.0_openmpi     | yes      | intel-18.0   | openmpi-3.0   | mkl 2018           | APW            | enable-netcdf-default                       |
|        |                | atlas_intel_19.1_bdir        | yes      | intel-19.1   | mpich-3.3.2   | mkl 2020           | APW            | build/ dir<br>enable-netcdf-default        |
| bob    | 1              | bob_gnu_13.2_openmp          | no       | gnu-13.2     |               | atlas-3.10         | P              | Fedora39 packages                           |
| buda2  | 1              | buda2_gnu_8.5_cuda           | odonly   | gnu-8.5      | openmpi-4.1.3 | mkl 2019.0.1<br>cuda 11.2 | | enable_gpu |
| eos    | 2              | eos_nvhpc_23.1_elpa          | yes      | nvhpc 23.1   | openmpi-3.0.5 |                    |                | cuda 12.2                                  |
|        |                | eos_nvhpc_23.9_elpa          | yes      | nvhpc 23.9   | openmpi-4.1.6 |                    |                | cuda 12                                    |
|        |                | eos_nvhpc_23.11_elpa         | no       | nvhpc 23.11  | openmpi-4.1.6 | netlib             | elpa 2022      |                                             |
|        |                | eos_nvhpc_23.9_hpc           | no       | nvhpc 23.9   | openmpi-4.1.6 | netlib_3.10.0      |                |                                             |
| higgs  | 1              | higgs_intel_19.0_serial      | yes      | intel-19.0   |               | mkl 2019           | APW            |                                             |
|        |                | higgs_gnu_12.3_cov           | odonly   | gnu-12.3     | mpich-4.1.2   | mkl 2019           | ABPW           | coverage analysis<br>enable-netcdf-default |
| scope  | 3              | scope_gnu_10.2_paral         | ref for tparal | gnu-10.2 | mpich-3.2 | OpenBLAS | BPW | mpirun -np 2 if max_nprocs allows it<br>enable-netcdf-default GW_SP |
|        |                | scope_gnu_13.2_dep           | yes      | gnu-13.2     | mpich-4.1.2   | OpenBLAS           | PW             | check dependency<br>enable-netcdf-default |
|        |                | scope_gnu_12.2_mpich         | yes      | gnu-12.2     | mpich-4.0.3   | OpenBLAS           | PW             | enable_memory_profiling                    |
|        |                | scope_gnu_12.2_abipy         | no       | gnu-12.2     | mpich-4.0.3   | OpenBLAS           | PW             | check abipy                                |
|        |                | scope_gnu_10.2_s64           | odonly   | gnu-10.2     | mpich-3.2     | OpenBLAS           | PW             | tutoparal with np=64                       |
| ubu    | 3              | ubu_gnu_9.2_openmpi          | yes      | gnu-9.2.0    | openmpi-4.0.2 | mkl 11.2           | APW            | check=all<br>-fno-frontend-optimize        |
|        |                | ubu_intel_16.0_mpich         | yes      | intel-16.0   | mpich-3.2     | mkl 11.3           | ABPW           |                                             |
|        |                | ubu_intel_16.0_openmp        | yes      | intel-16.0   |               | mkl 11.3           | A              | OpenMP / dfti                              |

*Caption for external fallbacks : A= AtomPAW, B= BigDFT, P= PSML+XMLF90, W= Wannier90

*Mandatory fallbacks : linalg, netCDF-C/netCDF-Fortran with HDF5 support and 
libXC 
