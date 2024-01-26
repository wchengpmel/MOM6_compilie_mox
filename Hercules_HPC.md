How to login Hercules HPC
```
ssh -Y viveks@hercules-login.hpc.msstate.edu
```
How to login Orion's HPC
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

