Notes on running HPL on a Raspberry Pi 3 (Model B)

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
