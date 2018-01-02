HPL

http://www.netlib.org/benchmark/hpl/

Download and build HPL.
~~~~
$ wget http://www.netlib.org/benchmark/hpl/hpl-2.1.tar.gz
$ tar zxvf hpl-2.1.tar.gz 
$ cd hpl-2.1/setup/
$ sh make_generic
~~~~

Customize HPL to use Atlas + Cortex compiler flags.

~~~~
$ cd ..
$ cp setup/Make.UNKNOWN Make.rpi
$ grep -v \# Make.rpi
SHELL        = /bin/sh
CD           = cd
CP           = cp
LN_S         = ln -s
MKDIR        = mkdir
RM           = /bin/rm -f
TOUCH        = touch
ARCH         = rpi
TOPdir       = $(HOME)/hpl-2.1
INCdir       = $(TOPdir)/include
BINdir       = $(TOPdir)/bin/$(ARCH)
LIBdir       = $(TOPdir)/lib/$(ARCH)
HPLlib       = $(LIBdir)/libhpl.a 
MPdir        = /home/rpimpi/mpich2-install
MPinc        = -I $(MPdir)/include
MPlib        = $(MPdir)/lib/libmpich.a
LAdir        = /usr/lib/atlas-base/
LAinc        = 
LAlib        = $(LAdir)/libf77blas.a $(LAdir)/libatlas.a
F2CDEFS      = -DAdd_ -DF77_INTEGER=int -DStringSunStyle
HPL_INCLUDES = -I$(INCdir) -I$(INCdir)/$(ARCH) $(LAinc) $(MPinc)
HPL_LIBS     = $(HPLlib) $(LAlib) $(MPlib)
HPL_OPTS     =
HPL_DEFS     = $(F2CDEFS) $(HPL_OPTS) $(HPL_INCLUDES) 
CC           = mpicc
CCNOOPT      = $(HPL_DEFS) 
CCFLAGS      = $(HPL_DEFS) -L /lib64 -lpthread
LINKER       = mpif77
LINKFLAGS    = -lpthread
ARCHIVER     = ar
ARFLAGS      = r
RANLIB       = echo
$ make arch=api
$ echo $?
$ file bin/rpi/xhpl
~~~~

Configure a test HPL.dat.
~~~
$ cat HPL.dat
HPLinpack benchmark input file
Innovative Computing Laboratory, University of Tennessee
HPL.out      output file name (if any)
6            device out (6=stdout,7=stderr,file)
1            # of problems sizes (N)
5120         Ns
1            # of NBs
128 256      NBs
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
~~~~

Run a test job.
~~~~
$ cat ~/mpi_testing/machinefile 
192.168.1.84
192.168.1.84
192.168.1.84
192.168.1.84
$ mpiexec -f ~/mpi_testing/machinefile -n 4 ./xhpl 
~~~~

## Runtime hints

For best performance, disable CPU frequency scaling, swap, and free unused RAM.
~~~
sudo -i
cat /sys/devices/system/cpu/cpu0/cpufreq/scaling_cur_freq
echo performance > /sys/devices/system/cpu/cpu0/cpufreq/scaling_governor
cat /sys/devices/system/cpu/cpu0/cpufreq/scaling_cur_freq 
sudo -i
echo 3 > /proc/sys/vm/drop_caches
free -k
swapoff -a
exit
~~~


## Helpful links

1. Cortex A53 technical specifications: http://infocenter.arm.com/help/topic/com.arm.doc.ddi0500d/DDI0500D_cortex_a53_r0p2_trm.pdf
2. Cortex A53 SIMD & floating point reference: https://static.docs.arm.com/ddi0502/f/DDI0502.pdf
3. GCC compiler flags for ARM: https://gcc.gnu.org/onlinedocs/gcc/ARM-Options.html#ARM-Options
