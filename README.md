This wrapper script is meant to be called via the starccm+ software via the -rsh flag.  
Its purpose is to separate the arguments for ssh and the commands sent to the remote system.
Once done it will insert the `singularity exec starccm.img` command into the ssh call to the system.

This is the command to execute starccm+ in parallel on all compute nodes in `nodelist`
```
singularity exec \
	/data/singularity/starccm.img \
	/opt/CD-adapco/12.04.011/STAR-CCM+12.04.011/star/bin/starccm+ \
	-power \
	-rsh ssh \
	-np 4 \
	-machinefile nodelist \
	-batch run \
	-load singleRow.sim
```

The problem with this is that when starccm+ uses its built in mpi to start jobs on the compute nodes, 
it does not know to start the singularity container on the compute node before calling starccm+.

To get around this, the above command needs to be changed to the following:
```
singularity exec \
	/data/singularity/starccm.img \
	/opt/CD-adapco/12.04.011/STAR-CCM+12.04.011/star/bin/starccm+ \
	-power \
	-rsh /PATH/TO/SSH-WRAPPER \
	-np 4 \
	-machinefile nodelist \
	-batch run \
	-load singleRow.sim
```
where the ssh-wrapper script is downloaded from this repository.

