# NWChem Compilation
### Abstract
I write this script to help and guide CentOS' user who want to install NWChem program. This repository includes shell script for auto compiling NWChem on CentOS 6.x &amp; 7.x. However, this script could be adjusted and adapted to perform on other Linux distribution. 

Additionally, after completing compilation, normally you should test program by running some calculation, such as the example calculation of optimization of azulene using DFT at M06-2X/6-31G(d) level of theory. Alright, it's such a friendly user script, very easy to use. The admin/user are expected to rapidly understand and be able to easily follow the instruction of my script through seccessive step (0) to (3).

### Requirement
* CentOS version 6.x or 7.x (or other Linux distro)
* PC or Laptop
* Keyboard
* Electric
* You and a pot of coffee

### Installation

  * **(0)**  Setting up. Make directory NWCHEM at /usr/local/src/ via sudo command or **root**. Check the required package *Before Compile*.
  * **(1)**  Download a program source of *nwchem-x.x.tar.gz* from NWChem website using **wget** command first. (I am using nwchem version 6.6 on the day I wrote this script). Save it at */usr/local/src/NWCHEM/* , then extract the program from tar file using *tar -xf nwchem-x.x.tar.gz*. So far you should have a nwchem-x.x directory in the present location.
  * **(2)**  Download a scripts to */usr/local/src/NWCHEM/nwchem-x.x*. Then run script [1_compile.sh](https://github.com/rangsimanketkaew/NWChem/blob/master/1_compile.sh). After finishing and there's no any error, then run [2_path.sh](https://github.com/rangsimanketkaew/NWChem/blob/master/2_path.sh), respectively. More instruction can be found in script [1_compile.sh](https://github.com/rangsimanketkaew/NWChem/blob/master/1_compile.sh). ! <br />
  * **(3)**  Up to now, let's move to your home directory. Then create a **.nwchemrc** file which includes following commands. Says, /home/$USER/.nwchemrc <br /> 
  * Noted that the day I created this manual and testing the compilation, I were using nwchem version 6.6 (However, this way should be work well with any 6.x & 7.x version of nwchem.
  
```
  nwchem_basis_library /usr/local/nwchem/data/libraries/
  nwchem_nwpw_library /usr/local/nwchem/data/libraryps/
  ffield amber
  amber_1 /usr/local/nwchem/data/amber_s/
  amber_2 /usr/local/nwchem/data/amber_q/
  amber_3 /usr/local/nwchem/data/amber_x/
  amber_4 /usr/local/nwchem/data/amber_u/
  spce    /usr/local/nwchem/data/solvents/spce.rst
  charmm_s /usr/local/nwchem/data/charmm_s/
  charmm_x /usr/local/nwchem/data/charmm_x/
```
---
***Optional: PATH SETTING.** Instead of running nwchem via direct path, you can make a alias path to call nwchem by using following command
```
export PATH=/usr/local/nwchem-6.6/bin/LINUX/nwchem:$PATH
```
If you want to call NWChem automatically for next time of log-in or ssh, each user have to use following command for appending the environment path of nwchem to their own $HOME/.bashrc file.
```
echo export PATH=/usr/local/nwchem-6.6/bin/LINUX/nwchem:$PATH >> /home/$USER/.bashrc
```
Then active the .bashrc file
```
source /home/$USER/.bashrc
```
Try to log-out and log-in again.

# Error recognition & fixing
During compilation using **make** or **configuration setting up** It is possible to meet an error in which caused by calling library mistake. <br />
E.g. *libmpi_f90.so.1: cannot open*, you have to use the following command to fix the issue.
```
export LD_LIBRARY_PATH=/usr/local/openmpi/lib/:$LD_LIBRARY_PATH
source $HOME/.bashrc
```
---
Since you run NWChem with MPI and suddenly meet the error like following
```
utilfname: cannot allocate
or
utilfname: cannot allocate:Received an Error in Communication
```
This error is telling that NWChem cannot allocate the memory with number of processors. The user have to specify the amount of memory **PER PROCESSOR CORE** that NWChem can use for a calculation. <br />
This issue can be easily fixed by adding a memory keyword into INPUT-FILE.nw, e.g.
```
memory 1 gb
```
If you run NWChem using, says, *"mpirun -np N nwchem INPUT-FILE.nw"* This mean that the total of used memory for this calculation is = (1 gb)*(N processors). <br />
However, safety first, you can limit the total of memory usage for calculation by specifying optional keyword of memory keyword, says
```
memory total 1 gb
```
More details about memory arrangement can be found this [website](http://www.nwchem-sw.org/index.php/Release66:Top-level#MEMORY)

# How to run easily NWChem
let's try to run nwchem with some test files from **/usr/local/src/NWCHEM/nwchem-6.6/examples/** or **/usr/local/src/NWCHEM/nwchem-6.6/QA/tests** by using the following command run
```
nohup mpirun -np N /usr/local/nwchem/bin/nwchem INPUT-FILE.nw >& OUTPUT-FILE.log
```
or
```
nohup mpirun -np N nwchem INPUT-FILE.nw >& OUTPUT-FILE.log
```
Just in case of MPI, the following command might be useful
```
export OMP_NUM_THREADS=N
```
where N = number of processors (integer & positive number).

## Before compile
The required packages of MPI variable compilation are following <br />
* OpenMPI (recommend OpenMPI 1.6.5) <br />
* Intel Compilers <br />
* Intel MKL <br />
* python-devel <br />
* gcc-gfortran <br />
* openblas-devel <br />
* openblas-serial64 <br />
* scalapack-openmpi-devel <br />
* elpa-openmpi-devel <br />
* tcsh <br />
* openssh-clients <br />
* which

## OpenMPI 1.6.5 Installation
[Please visit this website](http://lsi.ugr.es/~jmantas/pdp/ayuda/datos/instalaciones/Install_OpenMPI_en.pdf)

## OpenBLAS Installation
Source file [Download here](https://www.open-mpi.org/software/ompi/v1.6/) <br />
[Installation guide](https://github.com/xianyi/OpenBLAS/wiki/Installation-Guide)

## More details
Visit this wevsite [NWChem Compilation website](http://www.nwchem-sw.org/index.php/Compiling_NWChem) for more details.

## Contact info
You can contact me by e-mail: rangsiman1993(at)gmail.com (preferable) and rangsiman_k(at)sci.tu.ac.th (official).

## Enjoy !!!!
