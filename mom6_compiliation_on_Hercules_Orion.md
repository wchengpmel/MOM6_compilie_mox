## How to login Hercules HPC
```
ssh -Y viveks@hercules-login.hpc.msstate.edu
```
## How to login Orion HPC
```
ssh -Y viveks@orion-login.hpc.msstate.edu
```

## Downloading the source code of MOM6
```
git clone --recursive https://github.com/NOAA-GFDL/MOM6-examples.git MOM6-examples
```
That said, we strongly recommend having a GitHub account so you can version control your own modifications and contribute to MOM6.

The above will clone (download) the MOM6-examples repository and all its sub-modules into a directory called `MOM6-examples/`.

From here on, all commands will take place within the `MOM6-examples/` directory you just created.

### Comment
By way of explanation, the above one line "recursive clone" is equivalent to the follow steps:
```
git clone https://github.com/NOAA-GFDL/MOM6-examples.git MOM6-examples
cd MOM6-examples
git submodule init
git submodule update --recursive
```
## Compiling the model
We need to build different executables depending on the coupled mode of the model. All modes require MOM6 and FMS source code. When used in ice-ocean mode, whether with SIS or SIS2, we must link in a "null" land- and atmosphere-models. In fully coupled mode you also need the full land and atmosphere source codes.
The phases of compilation are:

1. Create a `path_names` file that contains the relative path to the source files. This uses the `list_paths` tools which ships with FRE but is also provided in the [mkmf](https://github.com/NOAA-GFDL/mkmf) package that you installed in the above [Build tools and compilation templates](#build-tools-and-compilation-templates) section.
1. Create a `Makefile` that will compile your library or executable. This uses the `mkmf` tool which ships with FRE.
1. Compile your library or executable.

We typically apply above steps either component-by-component (the usual method within FRE scripts) or, in these pages, applied to FMS and then everything else.

### Setting up the compile environment on Hercules
The following modules need to load before compiling MOM6 on Hercules
```
module load contrib/0.1
module load noaatools/3.1
module load rocoto
module use /work/noaa/fv3-cam/jabeles/share/modulefiles
module load  ufs_hercules.intel
```
### Setting up the compile environment on Orion
The following modules need to load before compiling MOM6 on Hercules
```
module load contrib/0.1
module load noaatools/3.1
module load rocoto
module use /work/noaa/fv3-cam/jabeles/share/modulefiles
module load  ufs_orion.intel
```
On both Hercules and Orion systems, MOM6 compiliation is same.

This one can be used on Hercules on `.bashrc` or `sbatch` script to export srun path `export LD_LIBRARY_PATH=/opt/slurm/lib/slurm:$LD_LIBRARY_PATH`

### Compiling the FMS shared code
It is best to compile the shared code separately from the model code (proves quicker when building multiple versions of the model).

Instructions for Hercules are shown below:
```
mkdir -p build/fms/
(cd build/fms; rm -f path_names; \
../../src/mkmf/bin/list_paths -l ../../src/FMS; \
../../src/mkmf/bin/mkmf -t ../../src/mkmf/templates/ncrc5-intel-main.mk -p libfms.a -c "-Duse_libMPI -Duse_netCDF" path_names)
```
On Hercules platform, used ncrc5-intel-main.mk template file, And need to change below mentioned lines
```
< ISA_OPT = -march=core-avx4     #add it
---  Remove below lines
> ifdef ISA
> ISA_OPT = $(ISA)
> ISA_REPRO = $(ISA)
> ISA_DEBUG = $(ISA)
> else
> ISA_OPT = -march=core-avx-i
> ISA_REPRO = -march=core-avx-i
> ISA_DEBUG = -march=core-avx-i
> endif
```
And also remove `-sox` from the template file.

The above creates the file build/fms/Makefile which you can use to build the fms library:
```
(cd build/fms/; make NETCDF=3 REPRO=1 libfms.a -j)
```
which creates the file build/fms/libfms.a. This is a library that we will link to after compiling the MOM6 source code.

If these step fails then it's very likely that your build template - "ncrc5-intel-main.mk" in this case - does not match your environment.

### Compiling MOM6 in MOM6-SIS2 coupled mode
This step is an alternative to the above "Compiling MOM6 without SIS2".
```
mkdir -p build/ice_ocean_SIS2/
(cd build/ice_ocean_SIS2/; rm -f path_names; \
../../src/mkmf/bin/list_paths -l ./ ../../src/MOM6/config_src/{infra/FMS1,memory/dynamic_symmetric,drivers/FMS_cap,external} ../../src/SIS2/config_src/dynamic_symmetric ../../src/MOM6/src/{*,*/*}/ ../../src/{atmos_null,coupler,land_null,ice_param,icebergs/src,SIS2,FMS/coupler,FMS/include}/)
(cd build/ice_ocean_SIS2/; \
../../src/mkmf/bin/mkmf -t ../../src/mkmf/templates/ncrc5-intel-main.mk -o '-I../fms' -p MOM6 -l '-L../fms -lfms' -c '-Duse_AM3_physics -D_USE_LEGACY_LAND_' path_names )
```
Finally, compile the MOM6 sea-ice ocean coupled model with:
```
(cd build/ice_ocean_SIS2/; make REPRO=1 MOM6 -j)
```
A successful compilation will yield the file `build/ice_ocean_SIS2/MOM6`.


