### Run Jupyter Notebook on Orion.
To connect to Orion's OnDemand interface, visit orion-ood.hpc.msstate.edu with a web browser. You must login via our CAS server with your HPC2 credentials and HPC2 Duo Authentication:
https://orion-ood.hpc.msstate.edu/

*For mote details https://intranet.hpc.msstate.edu/helpdesk/resource-docs/ood_guide.php*

#### Step1:
After connecting OnDemand
Click on `Jupyter` 

#### Step2:
It will ask account name, partition, QOS, Lab or Notebook
Fill in all details, but fill in JupyterLab

Python Kerenal
Additional Kernel Locations, colon separated
`$HOME/.local/share/jupyter/kernels`

Working Directory
`/work2/noaa/subarctic/viveks`

## How to manage Python Kerenl in Orion.
In the Orion Python and JupyterLab is coming with HPC, There is no Anaconda and also need install any environment, difficult.
Please follow the below steps to handle conda environments in JupyetrLab.
Install Anaconda on Orion specific to your account.
1. To download the installer, open a terminal and use the following command, depending on your Linux architecture.
   ```
   # Replace <INSTALLER_VERSION> with the version of the installer file you want to download
   # For example, https://repo.anaconda.com/archive/Anaconda3-2023.09-0-Linux-x86_64.sh
   # All installers can be found at repo.anaconda.com/archive/
   curl -O https://repo.anaconda.com/archive/Anaconda3-<INSTALLER_VERSION>-Linux-x86_64.sh
   ```
   View a full list of Anaconda Distribution installers at repo.anaconda.com/archive/. The latest installer versions are always at the top of the list.

2. To install, run the following command, depending on your Linux architecture:
   ```
   # Include the bash command even if you aren't using the Bash shell
   # Replace ~/Downloads with the path to the installer file, if necessary
   # Replace <INSTALLER_VERSION> with the version of the installer file
   # For example, Anaconda3-2023.09-0-Linux-x86_64.sh
   bash ~/Downloads/Anaconda3-<INSTALLER_VERSION>-Linux-x86_64.sh
   ```
3. Create conda environment for XESMF for MOM6,
   ```
    conda create -n xesmf_env
    conda activate xesmf_env
   ```
   Getting xESMF is as simple as:
   ```
   conda install -c conda-forge xesmf
   ```
   We also highly recommend those extra packages for full functionality:
   ```
   # to support all features in xESMF
    conda install -c conda-forge dask netCDF4
   # optional dependencies for executing all notebook examples
    conda install -c conda-forge matplotlib cartopy 
   ```
4. Install and activate ipykernel
   ```
   conda install -c conda-forge ipykernel
   ```
   ```
   python -m ipykernel install --user --name=xesmf_env
   ```
   





