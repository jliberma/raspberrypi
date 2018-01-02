
Notes on running HPL on a Raspberry Pi 3 (Model B)

  root@raspberrypi:~# lscpu
  Architecture:          armv7l
  Byte Order:            Little Endian
  CPU(s):                4
  On-line CPU(s) list:   0-3
  Thread(s) per core:    1
  Core(s) per socket:    4
  Socket(s):             1
  Model:                 4
  Model name:            ARMv7 Processor rev 4 (v7l)
  CPU max MHz:           1200.0000
  CPU min MHz:           600.0000
  BogoMIPS:              76.80
  Flags:                 half thumb fastmult vfp edsp neon vfpv3 tls vfpv4 idiva idivt vfpd32 lpae evtstrm crc32

  pi@raspberrypi:~/raspberrypi/hpl $ head /proc/meminfo 
  MemTotal:         949580 kB
  MemFree:          845704 kB
  MemAvailable:     852836 kB
  Buffers:            9092 kB
  Cached:            52268 kB
  SwapCached:            0 kB
  Active:            49220 kB
  Inactive:          27648 kB
  Active(anon):      15512 kB
  Inactive(anon):    12276 kB

  pi@raspberrypi:~/raspberrypi/hpl $ mpiexec --Version
  HYDRA build details:
      Version:                                 3.2
      Release Date:                            Wed Nov 11 22:06:48 CST 2015
      CC:                              gcc   -Wl,-z,relro 
      CXX:                             g++   -Wl,-z,relro 
      F77:                             gfortran  -Wl,-z,relro 
      F90:                             gfortran  -Wl,-z,relro 
      Configure options:                       '--disable-option-checking' '--prefix=/usr' '--build=arm-linux-gnueabihf' '--includedir=${prefix}/include' '--mandir=${prefix}/share/man' '--infodir=${prefix}/share/info' '--sysconfdir=/etc' '--localstatedir=/var' '--disable-silent-rules' '--libdir=${prefix}/lib/arm-linux-gnueabihf' '--libexecdir=${prefix}/lib/arm-linux-gnueabihf' '--disable-maintainer-mode' '--disable-dependency-tracking' '--enable-shared' '--enable-fortran=all' '--disable-rpath' '--disable-wrapper-rpath' '--sysconfdir=/etc/mpich' '--libdir=/usr/lib/arm-linux-gnueabihf' '--includedir=/usr/include/mpich' '--docdir=/usr/share/doc/mpich' '--with-hwloc-prefix=system' 'CPPFLAGS= -Wdate-time -D_FORTIFY_SOURCE=2 -I/mpich-3.2/src/mpl/include -I/mpich-3.2/src/mpl/include -I/mpich-3.2/src/openpa/src -I/mpich-3.2/src/openpa/src -D_REENTRANT -I/mpich-3.2/src/mpi/romio/include' 'CFLAGS= -g -O2 -fdebug-prefix-map=/mpich-3.2=. -fstack-protector-strong -Wformat -Werror=format-security -O2' 'CXXFLAGS= -g -O2 -fdebug-prefix-map=/mpich-3.2=. -fstack-protector-strong -Wformat -Werror=format-security -O2' 'FFLAGS= -g -O2 -fdebug-prefix-map=/mpich-3.2=. -fstack-protector-strong -O2' 'FCFLAGS= -g -O2 -fdebug-prefix-map=/mpich-3.2=. -fstack-protector-strong -O2' 'build_alias=arm-linux-gnueabihf' 'MPICHLIB_CFLAGS=-g -O2 -fdebug-prefix-map=/mpich-3.2=. -fstack-protector-strong -Wformat -Werror=format-security' 'MPICHLIB_CPPFLAGS=-Wdate-time -D_FORTIFY_SOURCE=2' 'MPICHLIB_CXXFLAGS=-g -O2 -fdebug-prefix-map=/mpich-3.2=. -fstack-protector-strong -Wformat -Werror=format-security' 'MPICHLIB_FFLAGS=-g -O2 -fdebug-prefix-map=/mpich-3.2=. -fstack-protector-strong' 'MPICHLIB_FCFLAGS=-g -O2 -fdebug-prefix-map=/mpich-3.2=. -fstack-protector-strong' 'LDFLAGS=-Wl,-z,relro' 'FC=gfortran' 'F77=gfortran' 'MPILIBNAME=mpich' '--cache-file=/dev/null' '--srcdir=.' 'CC=gcc' 'LIBS=-lpthread '
    Process Manager:                         pmi
    Launchers available:                     ssh rsh fork slurm ll lsf sge manual persist
    Topology libraries available:            hwloc
    Resource management kernels available:   user slurm ll lsf sge pbs cobalt
    Checkpointing libraries available:       blcr
    Demux engines available:                 poll select
