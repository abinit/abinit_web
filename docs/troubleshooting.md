
## Troubleshooting with ABINIT

Something going wrong ? Better to realize this than to think everything is OK ...

In this page, the user will find some tips to fix several problems with ABINIT.   

Many other tips might be found on the [forum](https://discourse.abinit.org), or in the 
[standard ABINIT documentation](https://docs.abinit.org).

These tips are not about configure or install problems, but about problems that can be fixed at the level of the input file written by the user.

### Identify the problem

Make sure you **get and read the error messages**. They are usually written at the end of the log file, and look like:

    --- !ERROR
    src_file: m_kg.F90
    src_line: 179
    mpi_rank: 1
    message:   (here follows the error message)

If you do not see such error message at the end of the log file, the error message might be found alternatively in an "error" file, depending on the command that you used to launch ABINIT.
Also, if you are running in batch, the behaviour might be different, and error messages might end up in another file than the log file.
If you run in parallel, the error messages might have been written in still another, separate file, entitled 

    __ABI_MPIABORTFILE__

If ABINIT stops quickly, consider to **run in sequential, interactively** (not in parallel neither in batch) to ease the understanding of what is happening.

Sometimes, the error message is prepared by another message giving preliminary information. Please, be sure to have identified the relevant information in the log file.

If you think to have obtained all information that ABINIT was supposed to give you, try to identify whether ABINIT stops because of geometry optimization problem or SCF problem. In the first case, your input geometry is perhaps crazy. Anyhow, see the next items in this troubleshooting page.

By the way, there might also be memory problems ... See <https://docs.abinit.org/topics/TuningSpeedMem>.


### Incorrect initial geometry 

Many mistakes done by beginners are related to incorrect starting geometry.

Here is a check list :

  * Check that the **units** are correct for your cell parameters and atomic positions. __ABINIT uses atomic unit by default__, but can use several other units if specified by the user, see [ABINIT input parameters](https://docs.abinit.org/guide/abinit/#parameters).
  * Check that the [typat](https://docs.abinit.org/variables/basic/#typat) atom types are correct with respect to [xred](https://docs.abinit.org/variables/basic/#xred) or [xcart](https://docs.abinit.org/variables/basic/#xcart).
  * Check that the **number of atoms** [natom](https://docs.abinit.org/variables/basic/#natom) is coherent with your list of coordinates, [xred](https://docs.abinit.org/variables/basic/#xred) or [xcart](https://docs.abinit.org/variables/basic/#xcart). __ABINIT reads only the coordinates of [natom](https://docs.abinit.org/variables/basic/#natom) nuclei, and **ignore others**__.
  * Relax first the atomic positions **at fixed primitive vectors** before optimizing the cell. Explicitly, use a first datadet with [optcell](https://docs.abinit.org/variables/rlx/#optcell) = 0, then a second dataset with non-zero [optcell](https://docs.abinit.org/variables/rlx/#optcell), in which you tell ABINIT to read optimized atomic positions using [getxred](https://docs.abinit.org/variables/rlx/#getxred) or [getxcart](https://docs.abinit.org/variables/rlx/#getxcart). In this second dataset, do not forget to use [dilatmx](https://docs.abinit.org/variables/rlx/#dilatmx) bigger than 1 if you expect the volume of the cell to increase during the optimization. Possibly after the atomic position relaxation, make a run with [chkdilatmx](https://docs.abinit.org/variables/rlx/#chkdilatmx)=0, then a third run with [dilatmx](https://docs.abinit.org/variables/rlx/#dilatmx)=1. See the additional suggestions in the documentation of [optcell](https://docs.abinit.org/variables/rlx/#optcell) . 
  * ABINIT can read VASP POSCAR external files containing unit cell parameters and atomic positions. See the input variable [structure](https://docs.abinit.org/variables/basic/#structure). This might help in setting the geometry correctly.
  * Try to visualize your primitive cell. There are numerous possibilities, using Abipy, VESTA, XCrysDen ...


### SCF cycle does not converge 

The default ABINIT algorithm for SCF (see input variable iscf) is an excellent compromise between speed and robustness.
It is referred to as Pulay II in Fig. 10 of
N. D. Woods and M. C. Payne and P. J. Hasnip, J. Phys.: Condens. Matter 31, 453001 (2019), and appear on the Pareto frontier.
Some parameters in the Kerker preconditioning have also an effect on the balance between speed and robustness, and the ABINIT default values
are good for metallic systems. As thouroughly discussed in this reference, there is no fool-proof fast algorithm.

If your SCF cycle do not converge (see nres2 or vres2 column in the output file), the reasons can be:
(1) Insufficient underlying accuracy in the solution of the Schrödinger at fixed potential
(2) Transient non-linear behaviour of the SCF, due either to (2a) sudden change of occupation numbers (usually only for metals), or (2b) long-wavelength fluctuations of potential, bigger than the gap.

Start first to address (2), by some tuning which can come without significantly slowing ABINIT. 
Test different values of [diemac](https://docs.abinit.org/variables/gstate/#diemac). 
The default is very large, and suitable for metals. Try running with a small value (perhaps 5) if your system is a bulk semiconductor or a molecule. If it is a doped solid, some intermediate value (perhaps 50) might be appropriate.

If this does not work, try to address (1), set 
[tolrde](https://docs.abinit.org/variables/dev/#tolrde) to 0.001, increase the value of 
[nline](https://docs.abinit.org/variables/gstet/#nline) (e.g. 6 or 8 instead of the default 4), as well as of 
[nnsclo](https://docs.abinit.org/variables/dev/#nnsclo). 
(e.g. 2 instead of the default). Your residm should be significantly lower than without such modifications, and perhaps your problem will be solved.

If this still does not work, but your residm did not look bad after all before addressing (1), 
then revert back to the default values of 
[tolrde](https://docs.abinit.org/variables/dev/#tolrde),
[nline](https://docs.abinit.org/variables/gstet/#nline),
and
[nnsclo](https://docs.abinit.org/variables/dev/#nnsclo), 
as this indeed slows down ABINIT.

Then, try to use 
[iscf](https://docs.abinit.org/variables/basic/#iscf) 2. 
This is a very slow but inconditionally convergent algorithm provided 
[diemix](https://docs.abinit.org/variables/gstate/#diemix)
is small enough. 
At some stage of convergence, you might restart with the obtained _DEN a better algorithm, 
as non-linear effects should have been eliminated at some stage.

More information about problems with SCF convergence is available at 
<https://github.com/abinit/abischool2026/blob/main/tuning_lecture_abischool2026.pdf> (42 slides).


