# Amarel Cluster Users Guide
This is a introductory guide on how to access and submit computing jobs to the Amarel cluster. For detailed infomation please read the offical guide:
https://sites.google.com/view/cluster-user-guide

## 1. Register for cluster access 
Register through https://oarc.rutgers.edu/amarel-cluster-access-request. Fill the form and usually you will be granted an account **your_netid@amarel.rutgers.com** within 72hrs. 

## 2. Visit thru Rutgers network or NO access
You have to be on Rutgers network to access the cluster and visit all the following links. This means you have to either physically present on campus and visit thru RUwireless_Secure network or using Rutgers VPN if you want to access the cluster remotely. 

For VPN, see the following page: https://soc.rutgers.edu/vpn/ 

In short, download the Cisco AnyConnect and connect to vpn.rutgers.com with your netid and pwd (and Duo Authentication).

## 2. GUI or SSH
You are allocated with 100Gb of storage that will never (I guess) be removed from the cluster. You will be installing and customizing your computing environment on the allocated storage space. There are multiple ways to request for computing resources from the cluster. If you prefer using a graphical user interface, then visit the following link:
https://ondemand.hpc.rutgers.edu/pun/sys/dashboard/batch_connect/sessions.

The easiest way is to create a Amarel Desktop instance and download_install_customize softwares and environments as you would on your PC (on Linux not Windows.)

If you are familar with Jupyter, you can create jupyter notebooks that runs on one of the cluster node with customized cpu cores and memory size. There is also an option to create an Rstudio_MATLAB server on the cluster if you are more familiar with R_MATLAB.

You can also access the cluster resources thru SSH. This will be your major way of submitting and monitoring jobs after you get your computing environment set up. Login to the cluster by the following shell script:
``` Shell
ssh netid@amarel.rutgers.edu
```

You will be asked for password, this will be the same as your netid password that you use to login to any Rutgers website. (Make sure that you are connecting thru Rutgers secure network.)

## 3. Login node
Now you are at the cluster login node through SSH. You will see a welcome message like this:
```
############################################################################

____ _  _ ____ ____ ____ _      ____ ___   ____ _  _ ___ ____ ____ ____ ____ 
|__| |\/| |__| |__/ |___ |      |__|  |    |__/ |  |  |  | __ |___ |__/ [__  
|  | |  | |  | |  \ |___ |___   |  |  |    |  \ |__|  |  |__] |___ |  \ ___]

  
        Welcome to the AMAREL research computing cluster managed by

   the Office of Advanced Research Computing (OARC) at Rutgers University

                          http://oarc.rutgers.edu

Need help? Send an e-mail to help@oarc.rutgers.edu

User documentation: https://sites.google.com/view/cluster-user-guide

Do NOT run programs, tests, analyses, or pre/postprocessing on the shared

login nodes (amarel1, amarel2, etc.). That's what compute nodes are for.
############################################################################
```

This is where you check the cluster availability, submit jobs, check job status, and do basically everything, EXCEPT FOR COMPUTING. No, not even a python "1+1". You are currently at the login node and the resources of this node will be used for login and job management for ALL users so a simple 1+1 is expensive. 

## 4. SLURM and useful commands
The computing jobs on Amarel cluster is managed by Slurm, a Linux workload management program. Below are some of the useful Slurm commands that you will be using everyday (run on login node).

* Check cluster status. 
``` Slurm
sinfo -s 
```

This will tell you how many computing nodes are online on the cluster, how many are in use, and how many are avaliable. You will see a message like this:
```
PARTITION AVAIL  TIMELIMIT   NODES(A/I/O/T)  NODELIST

main*        up 3-00:00:00   251/102/10/363  hal[0001-0288],slepner[009-048,054-088]

gpu          up 3-00:00:00        32/1/3/36  cuda[001-008],gpu[001-003,005-016],pascal[001-010],volta[001-003]

mem          up 3-00:00:00          4/2/0/6  mem[001-005],memnode001

cmain        up 3-00:00:00       18/50/0/68  halc[001-068]

cgpu         up 3-00:00:00          1/3/0/4  gpuc[001-004]

nonpre       up 3-00:00:00        5/11/0/16  hal[0099-0108,0125-0130]

graphical    up 1-00:00:00          0/1/0/1  sirius3
```

The "A_I_O_T" means "Allocated_Idle_Other_Total".  See https://slurm.schedmd.com/sinfo.html for details. Basically, if you see the number for "Idle" is zero then you may have to wait for next avaliable node. This usually happens at night because people like to wrap up whatever they've done to their code at daytime and throw it to the cluster before go to bed. If they don't then that's another 8hrs wasted. After all, the cluster doesn't need to sleep, while maybe yes but just once a month. 

* Check storage. See how much storage room you have used. 
``` 
mmlsquota —-block-size=auto cache
```

* Submit shell script jobs
``` Slurm
sbatch /scratch/my_netid/my_job.sh
```

Sync your job scripts to the cluster, usually to the scratch space (we will come back to talk about this later). After that, run above **sbatch** command to submit your job to the cluster. The requested resources will be automatically allocated to your job. 

* Check job status
``` Slurm
squeue -u my_netid 
```

Monitor the status of submitted jobs. Here you can see whether the requested resources are allocated and how long has your job been running. 

## 4.1 NOT Thru SSH: Sync files from/to the cluster (on local terminal) 
* DO NOT RUN ON LOGIN NODE
``` Shell
#! DO NOT RUN ON LOGIN NODE
rsync -trlvpz A_LOCAL_PATH netid@amarel.rutgers.edu:/scratch/my_netid

rsync -trlvpz netid@amarel.rutgers.edu:/scratch/my_netid/Results A_LOCAL_PATH
```



To be updated.  

July-25-2022







