# OpenBLAS

http://www.openblas.net/

OpenBLAS is an optimized BLAS library with ARM extensions.

It is based on the open source GotoBLAS2 library released by the [Texas Advanced Computing Center](https://www.tacc.utexas.edu/research-development/tacc-software/gotoblas2).

Download and compile.
~~~~
wget http://github.com/xianyi/OpenBLAS/archive/v0.2.20.tar.gz
tar zxvf v0.2.20.tar.gz
cd OpenBLAS
make
mkdir ../openblas-0.2.20
make PREFIX=/home/pi/openblas-0.2.20 install
~~~~

Test static and dynamic linking.
~~~
cd /home/pi/openblas-0.2.20
mkdir /home/pi/openblas-0.2.20/test
wget https://gist.githubusercontent.com/xianyi/5780018/raw/c1d93058a2f61b88b9dd4237d2cf4a963065070b/time_dgemm.c
gcc -o time_dgemm time_dgemm.c /home/pi/openblas-0.2.20/lib/libopenblas.a  -lpthread
./time_dgemm 1000 2000 4000
	3b. test dynamic linking
export LD_LIBRARY_PATH=/home/pi/openblas-0.2.20/lib
gcc -o time_dgemm time_dgemm.c -I /home/pi/openblas-0.2.20/include -L /home/pi/openblas-0.2.20/lib/ -lopenblas  -lpthread
ldd time_dgemm
./time_dgemm 1000 2000 4000
~~~~

Link to HPL
~~~~
SHELL        = /bin/sh
CD           = cd
CP           = cp
LN_S         = ln -s
MKDIR        = mkdir
RM           = /bin/rm -f
TOUCH        = touch
ARCH         = openblas
TOPdir       = $(HOME)/hpl-2.1
INCdir       = $(TOPdir)/include
BINdir       = $(TOPdir)/bin/$(ARCH)
LIBdir       = $(TOPdir)/lib/$(ARCH)
HPLlib       = $(LIBdir)/libhpl.a 
MPdir        = /home/rpimpi/mpich2-install
MPinc        = -I $(MPdir)/include
MPlib        = $(MPdir)/lib/libmpich.a
LAdir        = /home/pi/openblas-0.2.20
LAinc        = 
LAlib        = $(LAdir)/lib/libopenblas.a
F2CDEFS      = -DAdd_ -DF77_INTEGER=int -DStringSunStyle
HPL_INCLUDES = -I$(INCdir) -I$(INCdir)/$(ARCH) $(LAinc) $(MPinc)
HPL_LIBS     = $(HPLlib) $(LAlib) $(MPlib)
HPL_OPTS     =
HPL_DEFS     = $(F2CDEFS) $(HPL_OPTS) $(HPL_INCLUDES) 
CC           = mpicc
CCNOOPT      = $(HPL_DEFS) 
CCFLAGS      = $(HPL_DEFS) -L /lib64 -mcpu=cortex-a53 -mfloat-abi=hard -mfpu=neon-fp-armv8 -mneon-for-64bits -pthread 
LINKER       = mpif77
LINKFLAGS    = -pthread
ARCHIVER     = ar
ARFLAGS      = r
RANLIB       = echo
$ make arch=openblas
~~~~

Configure input file and run.
~~~
$ cat HPL.dat
HPLinpack benchmark input file
Innovative Computing Laboratory, University of Tennessee
HPL.out      output file name (if any)
6            device out (6=stdout,7=stderr,file)
1            # of problems sizes (N)
6912         Ns
1            # of NBs
144          NBs
0            PMAP process mapping (0=Row-,1=Column-major)
1            # of process grids (P x Q)
1 2          Ps
4 2          Qs
16.0         threshold
1            # of panel fact
2            PFACTs (0=left, 1=Crout, 2=Right)
1            # of recursive stopping criterium
4            NBMINs (>= 1)
1            # of panels in recursion
2            NDIVs
1            # of recursive panel fact.
1            RFACTs (0=left, 1=Crout, 2=Right)
1            # of broadcast
1            BCASTs (0=1rg,1=1rM,2=2rg,3=2rM,4=Lng,5=LnM)
1            # of lookahead depth
1            DEPTHs (>=0)
2            SWAP (0=bin-exch,1=long,2=mix)
64           swapping threshold
0            L1 in (0=transposed,1=no-transposed) form
0            U  in (0=transposed,1=no-transposed) form
1            Equilibration (0=no,1=yes)
8            memory alignment in double (> 0)
$ mpiexec -f ~/mpi_testing/machinefile -n 4 ./xhpl 
================================================================================
HPLinpack 2.1  --  High-Performance Linpack benchmark  --   October 26, 2012
Written by A. Petitet and R. Clint Whaley,  Innovative Computing Laboratory, UTK
Modified by Piotr Luszczek, Innovative Computing Laboratory, UTK
Modified by Julien Langou, University of Colorado Denver
================================================================================

An explanation of the input/output parameters follows:
T/V    : Wall time / encoded variant.
N      : The order of the coefficient matrix A.
NB     : The partitioning blocking factor.
P      : The number of process rows.
Q      : The number of process columns.
Time   : Time in seconds to solve the linear system.
Gflops : Rate of execution for solving the linear system.

The following parameter values will be used:

N      :    6912 
NB     :     144 
PMAP   : Row-major process mapping
P      :       1 
Q      :       4 
PFACT  :   Right 
NBMIN  :       4 
NDIV   :       2 
RFACT  :   Crout 
BCAST  :  1ringM 
DEPTH  :       1 
SWAP   : Mix (threshold = 64)
L1     : transposed form
U      : transposed form
EQUIL  : yes
ALIGN  : 8 double precision words

--------------------------------------------------------------------------------

- The matrix A is randomly generated for each test.
- The following scaled residual check will be computed:
      ||Ax-b||_oo / ( eps * ( || x ||_oo * || A ||_oo + || b ||_oo ) * N )
- The relative machine precision (eps) is taken to be               1.110223e-16
- Computational tests pass if scaled residuals are less than                16.0


================================================================================
T/V                N    NB     P     Q               Time                 Gflops
--------------------------------------------------------------------------------
WR11C2R4        6912   144     1     4             139.34              1.580e+00
HPL_pdgesv() start time Mon Jan  1 23:59:57 2018

HPL_pdgesv() end time   Tue Jan  2 00:02:16 2018

--------------------------------------------------------------------------------
||Ax-b||_oo/(eps*(||A||_oo*||x||_oo+||b||_oo)*N)=        0.0019737 ...... PASSED
================================================================================

Finished      1 tests with the following results:
              1 tests completed and passed residual checks,
              0 tests completed and failed residual checks,
              0 tests skipped because of illegal input values.
--------------------------------------------------------------------------------

End of Tests.
================================================================================
~~~

Calculate efficiency:

The 1200 MHz Cortex A53 is capable of 2.400+e00 floating point operations per second in double precision when using FMA instructions.

At 1.580e+00 Gflops, our efficiency is 0.6583.


## Helpful links:
1. http://www.openblas.net/
2. user guide: https://github.com/xianyi/OpenBLAS/wiki/User-Manual
3. install guide: https://github.com/xianyi/OpenBLAS/wiki/Installation-Guide
4. source code: https://github.com/xianyi/OpenBLAS
5. download: http://github.com/xianyi/OpenBLAS/archive/v0.2.20.tar.gz
