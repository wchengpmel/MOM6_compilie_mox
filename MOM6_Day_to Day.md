# How to Get an Intractive node in the MOX HYAK

## To get an interactive node with 4 cores in your own group for 2 hours on mox:

```
srun -p jisao -A jisao --nodes=1 --ntasks-per-node=4 --time=2:00:00 --mem=248G --pty /bin/bash
```

# How to padded jra55 reanalysis data for running MOM6 model output

Download the Python package from my Github repositories  ``` padded_JRA55do_v1.5.0 ```

```
git clone --recursive https://github.com/vseelanki/padded_JRA55do_v1.5.0.git padded
```
* pad files:
  
Create a conda environment using the provided env file

```
conda create --name pad_jra --file conda_env
cond activate pad_jra
```

Then create the padded files

```
./pad_files_JRA55do_v1.5.0.bash
```



