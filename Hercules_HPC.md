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

## Modules loading at Hercules System
```
module load contrib/0.1
module load rocoto
module load noaatools/3.1
module load netcdf-fortran/4.6.0
module load intel-oneapi-compilers/2022.2.1
```
```
module load openmpi/4.1.4
```

The below one needs to be added to the transcript and .bashrc
```
export LD_LIBRARY_PATH=/opt/slurm/lib/slurm:$LD_LIBRARY_PATH
```

## Compiling the FMS shared code at Herculus Compile system
```
mkdir -p build/fms/
(cd build/fms; rm -f path_names; \
../../src/mkmf/bin/list_paths -l ../../src/FMS; \
../../src/mkmf/bin/mkmf -t ../../src/mkmf/templates/ncrc5-intel-main.mk -p libfms.a -c "-Duse_libMPI -Duse_netCDF" path_names)
```
```
(cd build/fms/; make NETCDF=3 REPRO=1 libfms.a -j)
```
# Compiling MOM6 in MOM6-SIS2 coupled mode

```
mkdir -p build/ice_ocean_SIS2/
(cd build/ice_ocean_SIS2/; rm -f path_names; \
../../src/mkmf/bin/list_paths -l ./ ../../src/MOM6/config_src/{infra/FMS1,memory/dynamic_symmetric,drivers/FMS_cap,external} ../../src/SIS2/config_src/dynamic_symmetric ../../src/MOM6/src/{*,*/*}/ ../../src/{atmos_null,coupler,land_null,ice_param,icebergs/src,SIS2,FMS/coupler,FMS/include}/)
(cd build/ice_ocean_SIS2/; \
../../src/mkmf/bin/mkmf -t ../../src/mkmf/templates/ncrc5-intel-main.mk -o '-I../fms' -p MOM6 -l '-L../fms -lfms' -c '-Duse_AM3_physics -D_USE_LEGACY_LAND_' path_names )

```

```
(cd build/ice_ocean_SIS2/; make REPRO=1 MOM6 -j)
```


# Modules loading in ORION for MOM6 Compilation

```
module load impi/2020.2 intel/2020.2 netcdf/4.7.4
```

# Compiling the FMS shared code at ORION

Before compiling need changes nccs-intel.mk file
```
FC = mpiifort
CC = mpiicc
LD = mpiifort
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

# Compiling MOM6 in MOM6-SIS2 coupled mode

```
mkdir -p build/ice_ocean_SIS2/
(cd build/ice_ocean_SIS2/; rm -f path_names; \
../../src/mkmf/bin/list_paths -l ./ ../../src/MOM6/config_src/{infra/FMS1,memory/dynamic_symmetric,drivers/FMS_cap,external} ../../src/SIS2/config_src/dynamic_symmetric ../../src/MOM6/src/{*,*/*}/ ../../src/{atmos_null,coupler,land_null,ice_param,icebergs/src,SIS2,FMS/coupler,FMS/include}/)
(cd build/ice_ocean_SIS2/; \
../../src/mkmf/bin/mkmf -t ../../src/mkmf/templates/nccs-intel.mk -o '-I../fms' -p MOM6 -l '-L../fms -lfms' -c '-Duse_AM3_physics -D_USE_LEGACY_LAND_' path_names )
```
```
(cd build/ice_ocean_SIS2/; make REPRO=1 MOM6 -j)
```





