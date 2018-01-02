# Install MPICH2 on raspberry pi

http://www.southampton.ac.uk/~sjc/raspberrypi/pi_supercomputer_southampton.htm

Update and install gfortran.
~~~
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install gfortran
~~~~

Build mpich2.
~~~~
mkdir /home/pi/mpich2
cd ~/mpich2
wget http://www.mcs.anl.gov/research/projects/mpich2/downloads/tarballs/1.4.1p1/mpich2-1.4.1p1.tar.gz
tar xfz mpich2-1.4.1p1.tar.gz
sudo mkdir /home/rpimpi/mpich2-install
mkdir /home/pi/mpich_build
cd /home/pi/mpich_build
sudo /home/pi/mpich2/mpich2-1.4.1p1/configure -prefix=/home/rpimpi/mpich2-install
sudo make
echo $?
sudo make install
echo $?
~~~~

Test mpich2.
~~~~
export PATH=$PATH:/home/rpimpi/mpich2-install/bin
which mpicc
which mpiexec
cd ~
mkdir mpi_testing
cd /home/pi/mpi_testing/
ip addr show
cat machinefile 
192.168.1.84
192.168.1.84
192.168.1.84
192.168.1.84
mpiexec -f machinefile -n 4 hostname
mpiexec -f machinefile -n 2 ~/mpich_build/examples/cpi
~~~~
