Notes on running HPL on a Raspberry Pi 3 (Model B)

~~~~
================================================================================
T/V                N    NB     P     Q               Time                 Gflops
--------------------------------------------------------------------------------
WR11C2R4        8640   144     1     4             203.33              2.115e+00
HPL_pdgesv() start time Tue Jan  2 04:44:05 2018

HPL_pdgesv() end time   Tue Jan  2 04:47:28 2018

--------------------------------------------------------------------------------
||Ax-b||_oo/(eps*(||A||_oo*||x||_oo+||b||_oo)*N)=        0.0013903 ...... PASSED
================================================================================

Finished      1 tests with the following results:
              1 tests completed and passed residual checks,
              0 tests completed and failed residual checks,
              0 tests skipped because of illegal input values.
--------------------------------------------------------------------------------

End of Tests.
================================================================================
~~~

By my calculation, **%88** efficiency with a lot of tuning.

(2.115 Gflops, 2.4 theoretical in DP)

~~~~
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

  pi@raspberrypi:~/raspberrypi/hpl $ mpiexec --Version
  HYDRA build details:
      Version:                                 3.2
      Release Date:                            Wed Nov 11 22:06:48 CST 2015
      CC:                              gcc   -Wl,-z,relro 
      CXX:                             g++   -Wl,-z,relro 
      F77:                             gfortran  -Wl,-z,relro 
      F90:                             gfortran  -Wl,-z,relro 
~~~~
