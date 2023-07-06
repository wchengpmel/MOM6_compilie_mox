# Python jupyter notebook on mox hyak
## On a mox login node: *

Get an interactive node: (xyz is the group name and abc is the userid).

Here example: ```xyz=jisao``` and ```abc=vivek```
```

[vivek@mox1 ~]$ srun -p jisao --time=2:00:00 --mem=100G --pty /bin/bash
```

(Suppose the node hostname is n1234 and so the node number is 1234.)
```
jupyter notebook --no-browser --port=8899
```

## On your desktop: *

Replace abc by your hyak userid:
```
ssh -N -f -L 127.0.0.1:8899:127.0.0.1:8899 abc@mox.hyak.uw.edu
```

## On another hyak login node : *

Replace 1234 by the interactive node number:
```
ssh -N -f -L 127.0.0.1:8899:127.0.0.1:8899 n1234
```

## On your desktop: *
 
 Point your browser to:

http://127.0.0.1:8899

I recommend referencing the [WIKI for hyak users](https://wiki.cac.washington.edu/display/hyakusers/Mox_ipython_jupyter)  
