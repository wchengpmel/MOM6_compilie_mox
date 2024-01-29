## How to login Hercules HPC
```
ssh -Y viveks@hercules-login.hpc.msstate.edu
```
## How to login Orion's HPC
```
ssh -Y viveks@orion-login.hpc.msstate.edu
```
For Getting interactive node Hercules
```
srun --nodes 2 --ntasks-per-node 4 -A subarctic --time=00:10:00  --pty /bin/bash
```
```
srun --nodes 1 --ntasks-per-node 1 --time=00:10:00  --pty /bin/bash
```
``` Note: These Interactive nodes have no Internet connection ```

## How to install Anaconda on Hercules or Orion
```
curl -O https://repo.anaconda.com/archive/Anaconda3-2023.09-0-Linux-x86_64.sh
```

```
mkdir -p build/fms/
(cd build/fms; rm -f path_names; \
../../src/mkmf/bin/list_paths -l ../../src/FMS; \
../../src/mkmf/bin/mkmf -t ../../src/mkmf/templates/nccs-intel.mk -p libfms.a -c "-Duse_libMPI -Duse_netCDF" path_names)
```

```
(cd build/fms/; make NETCDF=3 REPRO=1 libfms.a -j)
```

nccs-intel.mk

```
Currently Loaded Modules:
  1) contrib/0.1   2) rocoto/1.3.5   3) intel-oneapi-compilers/2022.2.1   4) slurm/22.05.8   5) ucx/1.13.1   6) mpich/4.0.2   7) netcdf-fortran/4.6.0
```
