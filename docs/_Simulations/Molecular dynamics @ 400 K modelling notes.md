---
layout: post
title:  MAPI + proton AIMD @ 400 K
author: Lei Lei
date:   2022-04-02 14:34:07 +0100
category: Lab Notes
---

This note records the ab initio molecular dynamics (AIMD) simulation job run on the UK tier2 HPC young. 

The scripts are prepared by Sanliang Ling.

The job.pbs script modified by Lei is as follow:

```shell
#!/bin/bash -l

# Batch script to run an MPI parallel job under SGE with Intel MPI.

# To specify a project ID in a job script
#$ -P Gold
#$ -A MCC_surfin_lin

# Request 48 hours of wallclock time (format hours:minutes:seconds).
#$ -l h_rt=48:00:00

# Request 4 gigabyte of RAM per process (must be an integer followed by M, G, or T)
#$ -l mem=4G

# Set the name of the job.
#$ -N CP2K

# Select the MPI parallel environment and 16 processes.
#$ -pe mpi 120

# Set the working directory to somewhere in your scratch space.
#$ -wd /home/mmm0978/Scratch/cp2k/mapi/nvt-400K

# Load relevant modules
module unload default-modules/2018
module load gerun
module load compilers/gnu/4.9.2 openblas/0.3.7-openmp/gnu-4.9.2
module load mpi/openmpi/3.1.4/gnu-4.9.2
module load cp2k

# Run our MPI job.  GERun is a wrapper that launches MPI jobs on our clusters.
gerun /shared/ucl/apps/cp2k/7.1/gnu-4.9.2/cp2k-7.1/bin/cp2k.popt sys.inp > sys.out
```

### 10 ps

The job was submitted at 10 May 2021 by Lei

The screen shot of job status:

![](../nvt-400K/65b721b4-a7ff-497d-a386-75b7d9e53525.png)

The job starts to run at 12:41, 10 May 2021:

![](../nvt-400K/31c88434-d1b3-46f7-ae90-84fc8e105e2a.png)

The job was done at 13:30 11 May 2021.

-------------------------------------------------------------------------------
 **** **** ******  **  PROGRAM ENDED AT                 2021-05-11 13:30:26.857
 ***** ** ***  *** **        PROGRAM RAN ON                             node-c12d-028
 **    ****   ******     PROGRAM RAN BY                                   mmm0978
 ***** **    ** ** **    PROGRAM PROCESS ID                                 17026
 **** **  *******  **   PROGRAM STOPPED IN /lustre/scratch/mmm0978/cp2k/mapi/nvt
                                           -400K

### 20 ps

This job adds another 20000 AIMD steps (10 ps) to the existing trajectory. 

1. Back up sys.inp & sys.out files:

   ```
   cp sys.inp sys.0514.inp
   ```

   ![backup inp file](../nvt-400K/backup inp file.png)

   ```
   cp sys.out sys.0514.out
   ```

   ![backup out file](../nvt-400K/backup out file.png)

2. Make new sys.inp file from the sys-1.restart

   ```shell
   cp sys-1.restart sys.inp
   ```

   ![make new inp file from restart](../nvt-400K/make new inp file from restart.png)

3. Submit the job again:

   ```
   qsub job.pbs
   ```

   <img src="../nvt-400K/jobsub.png" alt="jobsub" style="width:60%;" />

   Check job status:

   ```
   qstat
   ```

   <img src="../nvt-400K/job299171stat.png" alt="job299171stat" style="width:60%;" />

   Job job was done at 15:34 15 May 2021:

   -------------------------------------------------------------------------------
     **** **** ******  **  PROGRAM ENDED AT                 2021-05-15 15:34:42.398
    ***** ** ***  *** **   PROGRAM RAN ON                             node-c12d-028
    **    ****   ******    PROGRAM RAN BY                                   mmm0978
    ***** **    ** ** **   PROGRAM PROCESS ID                                 40122
     **** **  *******  **  PROGRAM STOPPED IN /lustre/scratch/mmm0978/cp2k/mapi/nvt
                                              -400K

   ### 30 ps

   Budgets before simulation:

   <img src="../nvt-400K/budget_before.png" alt="budget_before" style="width:60%;" />

   1. Backup sys.inp & sys.out files

      ```
      cp sys.inp sys_42000_1505.inp && cp sys.out sys_42000_1505.out
      ```

      <img src="../nvt-400K/backup_inp_out_1505.png" alt="backup_inp_out_1505" style="width:60%;" />

      <img src="../nvt-400K/backup_1505.png" alt="backup_1505" style="width:60%;" />

   2. Make new sys.inp file from sys-1.restart file

      ```shell
      cp sys-1.restart sys.inp
      ```

      <img src="../nvt-400K/newINP_1505.png" alt="newINP_1505" style="width:60%;" />

      Check the new sys.inp file, the STEP_START_VAL should be 42000:

      ```Shell
      nano sys.inp
      ```

      

      <img src="../nvt-400K/check inp_1505.png" alt="check inp_1505" style="width:60%;" />

   3. Submit the job again by the command:

      ```Shell
      qsub job.pbs
      ```

      <img src="../nvt-400K/1505 qsub.png" alt="1505 qsub" style="width:60%;" />

      Check job status:

      ```
      qstat
      ```

      <img src="../nvt-400K/Screenshot 2021-05-15 181135_qstat.png" alt="Screenshot 2021-05-15 181135_qstat" style="width:60%;" />

      The job starts to run at 18: 12 15 May.

      <img src="../nvt-400K/Screenshot 2021-05-15 181428_qstat.png" alt="Screenshot 2021-05-15 181428_qstat" style="width:60%;" />

   ### 40 ps

   Budgets before simulation:

   <img src="../nvt-400K/40_ps_budgets.png" alt="40_ps_budgets" style="width:60%;" />

   1. Backup sys.inp & sys.out files

      Open the directory:

      ```
      cd Scratch/cp2k/mapi/nvt-400K
      ```

      Backup files:

      ```
      cp sys.inp sys_62000_1805.inp && cp sys.out sys_62000_1805.out
      ```
      <img src="../nvt-400K/40_ps_backup.png" alt="40_ps_backup" style="width:60%;" />

      <img src="../nvt-400K/40_ps_backup_check.png" alt="40_ps_backup_check" style="width:60%;" />

   2. Make new sys.inp file from sys-1.restart file

      ```
      cp sys-1.restart sys.inp
      ```

      <img src="../nvt-400K/40_ps_make_inp.png" alt="40_ps_make_inp" style="width:60%;" />

   3. Check the new sys.inp file, the STEP_START_VAL should be 62000:

      ```
      nano sys.inp
      ```

      <img src="../nvt-400K/40_ps_check_inp.png" alt="40_ps_check_inp" style="width:60%;" />

   4. Submit the job again by the command:

      ```
      qsub job.pbs
      ```

      <img src="../nvt-400K/40_ps_subjob.png" alt="40_ps_subjob" style="width:60%;" />

      Check job status:

      ```
      qstat
      ```

      <img src="../nvt-400K/40_ps_qstat1.png" alt="40_ps_qstat1" style="width:60%;" />

      The job starts to run at 11: 42 18 May.

      <img src="../nvt-400K/40_ps_running.png" alt="40_ps_running" style="width:60%;" />

### 50 ps

1. Backup sys.inp & sys.out files

   Open the directory:

   ```
   cd Scratch/cp2k/mapi/nvt-400K
   ```

   Backup files:

   ```
   cp sys.inp sys_82000_2005.inp && cp sys.out sys_82000_2005.out
   ```

   <img src="../nvt-400K/50_ps_backup.png" alt="50_ps_backup" style="width:60%;" />

   <img src="../nvt-400K/50_ps_backup_check.png" alt="50_ps_backup_check" style="width:60%;" />

2. Make new sys.inp file from sys-1.restart file

   ```
   cp sys-1.restart sys.inp
   ```

   <img src="../nvt-400K/50_ps_make_inp.png" alt="50_ps_make_inp" style="width:60%;" />

3. Check the new sys.inp file, the STEP_START_VAL should be 82000:

   ```
   nano sys.inp
   ```

   <img src="../nvt-400K/50_ps_check_inp.png" alt="50_ps_check_inp" style="width:60%;" />

4. Submit the job again by the command:

   ```
   qsub job.pbs
   ```

   <img src="../nvt-400K/50_ps_subjob.png" alt="50_ps_subjob" style="width:60%;" />

   Check job status:

   ```
   qstat
   ```

   <img src="../nvt-400K/50_ps_qstat1.png" alt="50_ps_qstat1" style="width:60%;" />

   The job starts to run at 11: 42 18 May.

   <img src="../nvt-400K/50_ps_running.png" alt="50_ps_running" style="width:60%;" />

### 60 ps

1. Backup sys.inp & sys.out files

   Open the directory:

   ```
   cd Scratch/cp2k/mapi/nvt-400K
   ```

   Backup files:

   ```
   cp sys.inp sys_102000_2205.inp && cp sys.out sys_102000_2205.out
   ```

   <img src="../nvt-400K/60_ps_backup.png" alt="60_ps_backup" style="width:60%;" />

   <img src="../nvt-400K/60_ps_backup-check.png" alt="60_ps_backup-check" style="width:60%;" />

2. Make new sys.inp file from sys-1.restart file

   ```
   cp sys-1.restart sys.inp
   ```

   <img src="../nvt-400K/60_ps_make_inp.png" alt="60_ps_make_inp" style="width:60%;" />

3. Check the new sys.inp file, the STEP_START_VAL should be 102000:

   ```
   nano sys.inp
   ```

   <img src="../nvt-400K/60_ps_check_inp.png" alt="60_ps_check_inp" style="width:60%;" />

4. Submit the job again by the command:

   ```
   qsub job.pbs
   ```

   <img src="../nvt-400K/60_ps_subjob.png" alt="60_ps_subjob" style="width:60%;" />

   Check job status:

   ```
   qstat
   ```

   <img src="../nvt-400K/60_ps_qstatqw.png" alt="60_ps_qstatqw" style="width:60%;" />

   The job starts to run at 23: 42 22 May.

   <img src="../nvt-400K/60_ps_running.png" alt="60_ps_running" style="width:60%;" />

### 70 ps

1. Backup sys.inp & sys.out files

   Open the directory:

   ```
   cd Scratch/cp2k/mapi/nvt-400K
   ```

   Backup files:

   ```
   cp sys.inp sys_122000_2705.inp && cp sys.out sys_122000_2705.out
   ```

   <img src="../nvt-400K/70_ps_backup.png" alt="70_ps_backup" style="width:60%;" />

   <img src="../nvt-400K/70_ps_backup-check.png" alt="70_ps_backup-check" style="width:60%;" />

2. Make new sys.inp file from sys-1.restart file

   ```
   cp sys-1.restart sys.inp
   ```

   <img src="../nvt-400K/70_ps_make_inp.png" alt="70_ps_make_inp" style="width:60%;" />

3. Check the new sys.inp file, the STEP_START_VAL should be 122000:

   ```
   nano sys.inp
   ```

   <img src="../nvt-400K/70_ps_check_inp.png" alt="70_ps_check_inp" style="width:60%;" />

4. Submit the job again by the command:

   ```
   qsub job.pbs
   ```

   <img src="../nvt-400K/70_ps_subjob.png" alt="70_ps_subjob" style="width:60%;" />

   Check job status:

   ```
   qstat
   ```

   The job starts to run at 14:23, 27 May.

   <img src="../nvt-400K/70_ps_running.png" alt="70_ps_running" style="width:60%;" />
### 80 ps

1. Backup sys.inp & sys.out files

   Open the directory:

   ```
   cd Scratch/cp2k/mapi/nvt-400K
   ```

   Backup files:

   ```
   cp sys.inp sys_142000_1807.inp && cp sys.out sys_142000_1807.out && ls
   ```

   <img src="../nvt-400K/80ps_backup.png" alt="80ps_backup" style="width:60%;" />

   <img src="../nvt-400K/70_ps_backup-check.png" alt="70_ps_backup-check" style="width:60%;" />

2. Make new sys.inp file from sys-1.restart file

   ```
   cp sys-1.restart sys.inp
   ```

   <img src="../nvt-400K/80ps_make_inp.png" alt="80ps_make_inp" style="width:60%;" />

3. Check the new sys.inp file, the STEP_START_VAL should be 142000:

   ```
   nano sys.inp
   ```

   <img src="../nvt-400K/80ps_check_inp.png" alt="80ps_check_inp" style="width:60%;" />

4. Submit the job again by the command:

   ```
   qsub job.pbs
   ```

   <img src="../nvt-400K/80ps_qsub.png" alt="80ps_qsub" style="width:60%;" />

   Check job status:

   ```
   qstat
   ```

   The job starts to run at 15:55, 18 July.

   <img src="../nvt-400K/80ps_qw.png" alt="80ps_qw" style="width:60%;" />

   <img src="../nvt-400K/80ps_running.png" alt="80ps_running" style="width:60%;" />
### 90 ps

1. Backup sys.inp & sys.out files

   Open the directory:

   ```
   cd Scratch/cp2k/mapi/nvt-400K
   ```

   Backup files:

   ```
   cp sys.inp sys_162000_2007.inp && cp sys.out sys_162000_2007.out && ls
   ```

   <img src="../nvt-400K/90ps_backup.png" alt="90ps_backup" style="width:60%;" />

   <img src="../nvt-400K/90ps_backup_check.png" alt="90ps_backup_check" style="width:60%;" />

2. Make new sys.inp file from sys-1.restart file

   ```
   cp sys-1.restart sys.inp && nano sys.inp
   ```

   <img src="../nvt-400K/90ps_make_inp.png" alt="90ps_make_inp" style="width:60%;" />

3. Check the new sys.inp file, the STEP_START_VAL should be 162000:

   <img src="../nvt-400K/90ps_check_inp.png" alt="90ps_check_inp" style="width:60%;" />
   
4. Submit the job again by the command:

   ```
   qsub job.pbs
   ```

   <img src="../nvt-400K/90ps_qsub.png" alt="90ps_qsub" style="width:60%;" />

   Check job status:

   ```
   qstat
   ```

   The job starts to run at 15:55, 18 July.

   <img src="../nvt-400K/90ps_qw.png" alt="90ps_qw" style="width:60%;" />

   <img src="../nvt-400K/90ps_running.png" alt="90ps_running" style="width:60%;" />
### 100 ps

1. Backup sys.inp & sys.out files

   Open the directory:

   ```
   cd Scratch/cp2k/mapi/nvt-400K
   ```

   Backup files:

   ```
   cp sys.inp sys_182000_2007.inp && cp sys.out sys_182000_2007.out && ls
   ```

   <img src="../nvt-400K/100ps_backup.png" alt="100ps_backup" style="width:60%;" />

   <img src="../nvt-400K/100ps_backup_check.png" alt="100ps_backup_check" style="width:60%;" />

2. Make new sys.inp file from sys-1.restart file

   ```
   cp sys-1.restart sys.inp && nano sys.inp
   ```

   <img src="../nvt-400K/100ps_make_inp.png" alt="100ps_make_inp" style="width:60%;" />

3. Check the new sys.inp file, the STEP_START_VAL should be 182000:

   <img src="../nvt-400K/100ps_check_inp.png" alt="100ps_check_inp" style="width:60%;" />

4. Submit the job again by the command:

   ```
   qsub job.pbs
   ```

   <img src="../nvt-400K/100ps_qsub.png" alt="100ps_qsub" style="width:60%;" />

   Check job status:

   ```
   qstat
   ```

   The job starts to run at 15:55, 18 July.

   <img src="../nvt-400K/100ps_qw.png" alt="100ps_qw" style="width:60%;" />

   <img src="../nvt-400K/90ps_running.png" alt="90ps_running" style="width:60%;" />
   
   The running stopped, try to continue.
   
   1. Backup files:
   
      ```
      cp sys.inp sys_184000_2307.inp && cp sys.out sys_184000_2307.out && ls
      ```
   
      <img src="../nvt-400K/100ps_backup.png" alt="100ps_backup" style="width:60%;" />
   
      <img src="../nvt-400K/100ps_backup_check.png" alt="100ps_backup_check" style="width:60%;" />
   
   2. Make new sys.inp file from sys-1.restart file
   
      ```
      cp sys-1.restart sys.inp && nano sys.inp
      ```
   
      <img src="../nvt-400K/100ps_make_inp.png" alt="100ps_make_inp" style="width:60%;" />
   
   3. Check the new sys.inp file, the STEP_START_VAL should be 184220:
   
      <img src="../nvt-400K/100ps_check_inp.png" alt="100ps_check_inp" style="width:60%;" />
   
   4. Submit the job again by the command:
   
      ```
      qsub job.pbs
      ```
   
      <img src="../nvt-400K/100ps_qsub.png" alt="100ps_qsub" style="width:60%;" />
   
      Check job status:
   
      ```
      qstat
      ```
   
      The job starts to run at 15:55, 18 July.
   
      <img src="../nvt-400K/100ps_qw.png" alt="100ps_qw" style="width:60%;" />
   
      <img src="../nvt-400K/90ps_running.png" alt="90ps_running" style="width:60%;" />
      
      The job did not run due to the low budgets. The steps were reduced, and the job was submitted again:
      
      ```shell
      nano sys.inp
      ```
      
      <img src="../nvt-400K/95ps_check_inp.png" alt="95ps_check_inp" style="width:60%;" />
      
      ```shell
      qsub job.pbs
      ```
      
      <img src="../nvt-400K/95ps_qsub.png" alt="95ps_qsub" style="width:60%;" />
      
      ```bash
      qstat
      ```
      
      <img src="../nvt-400K/95ps_qw.png" alt="95ps_qsub" style="width:60%;" />
   
   The job did not run due to insufficient cpu hours.
   
   The 100 ps job was continued on Cirrus. The submission script used is `job.sh`:

```shell
#!/bin/bash -l

# Slurm job options (name, compute nodes, job time)
#SBATCH --job-name=CP2K_400K_100ps
#SBATCH --time=36:00:00
#SBATCH --exclusive
#SBATCH --nodes=4
#SBATCH --tasks-per-node=36
#SBATCH --cpus-per-task=1

# Replace [budget code] below with your budget code (e.g. t01)
#SBATCH --account=ec175
# We use the "standard" partition as we are running on CPU nodes
#SBATCH --partition=standard
# We use the "standard" QoS as our runtime is less than 4 days
#SBATCH --qos=standard

# Load cp2k
module load cp2k

# Change to the submission directory
cd $SLURM_SUBMIT_DIR

#Ensure that no libraries are inadvertently using threading
export OMP_NUM_THREADS=1

# Launch the parallel job
srun cp2k.popt -i sys.inp > sys.out
```

The `sys.inp` file was copied from Young. 

<img src="../nvt-400K/cirrus100ps_check_inp.png" alt="cirrus100ps_check_inp" style="width:65%;" />



Submit the job by:

```
sbatch job.slurm
```

<img src="../nvt-400K/cirrus100ps_sub_name_error.png" alt="cirrus100ps_sub_name_error" style="width:65%;" />

This is due to a text editor encoding the `job.slurm` batch script using DOS-style line breaks. This often occurs when creating or editing a script on a Windows system. This error can be solved by converting the format to unix style by:

```shell
dos2unix job.slurm
```

<img src="../nvt-400K/cirrus100ps_formatting.png" alt="cirrus100ps_formatting" style="width:65%;" />

The job was submitted again and another error occurred:

<img src="../nvt-400K/cirrus100ps_filename_error.png" alt="cirrus100ps_filename_error" style="width:65%;" />

This is due to leading whitespace is not supported in SLURM job names. The job will be rejected with an error message if you submit a job with a space in the job name. This was resolved by deleting the space in the beginning of the job name. The job was submitted once again:

<img src="../nvt-400K/cirrus100ps_sbatch.png" alt="cirrus100ps_sbatch" style="width:65%;" />

THe job status can be checked with the command:

```shell
squeue
```

<img src="../nvt-400K/cirrus100ps_squeue_head.png" alt="cirrus100ps_squeue_head" style="width:65%;" />

<img src="../nvt-400K/cirrus100ps_squeue_pd.png" alt="cirrus100ps_squeue_pd" style="width:65%;" />

The job was not run correctly due to the large wavefunction file was not uploaded.

The job was submitted again after uploading alls the files on Young.

```
sbatch job.slurm && squeue -u uonlei
```

<img src="../nvt-400K/cirrus100ps_sub_again1308.png" alt="cirrus100ps_sub_again1308" style="width:65%;" />

Still the job did not run correctly, the job was cancelled.

```
scancel 1650307
```



The position, force and velocity data were downloaded from the HPC and the :

Backup the files and delete the information wrote after 182000 steps.

Upload the files that has 182000 steps’ data and run again.

The job did not run due to the wavefunction file to restart the job is not complete. Use the backup file to continue the job:

```shell
cp sys-RESTART.wfn.bak-1 sys-RESTART.wfn
```

<img src="../nvt-400K/100ps_make_wfn.png" alt="100ps_make_wfn" style="width:65%;" />

Submit the job again on Cirrus:

```shell
sbatch job.slurm && squeue -u uonlei
```

<img src="../nvt-400K/100ps_sbatch.png" alt="100ps_sbatch" style="width:65%;" />

### 110 ps

1. Backup sys.inp & sys.out files.

   Open the directory:

   ```
   cd ~/mapi/nvt-400K
   ```

   Backup files:

   ```
   cp sys.inp sys_202000_1009.inp && cp sys.out sys_202000_1009.out && ls
   ```

   <img src="../nvt-400K/110ps_backup.png" alt="110ps_backup" style="width:60%;" />

   <img src="../nvt-400K/110ps_backup_check.png" alt="110ps_backup_check" style="width:60%;" />

2. Make new sys.inp file from sys-1.restart file

   ```
   cp sys-1.restart sys.inp && nano sys.inp
   ```

   <img src="../nvt-400K/110ps_make_inp.png" alt="110ps_make_inp" style="width:60%;" />

3. Check the new sys.inp file, the STEP_START_VAL should be 202000:

   <img src="../nvt-400K/110ps_check_inp.png" alt="110ps_check_inp" style="width:60%;" />

4. Submit the job again by the command:

   ```
   sbatch job.slurm && squeue -u uonlei
   ```

   <img src="../nvt-400K/110ps_sbatch.png" alt="100ps_sbatch" style="width:60%;" />

### 120 ps

   Use the `resub.slurm` script to automatically submit the job.

   ```shell
   #!/bin/bash
   
   # Slurm job options (name, compute nodes, job time)
   #SBATCH --job-name=CP2K_400K
   #SBATCH --time=36:00:00
   #SBATCH --exclusive
   #SBATCH --nodes=4
   #SBATCH --tasks-per-node=36
   #SBATCH --cpus-per-task=1
   
   # Replace [budget code] below with your budget code (e.g. t01)
   #SBATCH --account=ec175
   # We use the "standard" partition as we are running on CPU nodes
   #SBATCH --partition=standard
   # We use the "standard" QoS as our runtime is less than 4 days
   #SBATCH --qos=standard
   
   # Get all the filenames end with .restart and store the filename into an array
   i=0
   while read line
   do
       array[ $i ]="$line"
       (( i++ ))
   done < <(find -name "*.restart")
   
   # Looping through the array and extract the steps from the filename
   for ((i=0; i<${#array[*]}; i++));
   do
        str=${array[i]}
        str=${str##*_}
        array[ $i ]=${str%%.*}
   done
   
   # Find the maximum number in the steps array which is the current simulation step
   current_step=${array[0]}
   for n in "${array[@]}" ; do
       ((n > current_step)) && current_step=$n
   done
   
   # Get the date and make the backup names for sys.inp and sys.out
   now=$(date +'%d-%m-%Y')
   backup_inp_name='sys-'${current_step}'-'${now}'.inp'
   backup_out_name='sys-'${current_step}'-'${now}'.out'
   # echo "${backup_inp_name}"
   # echo "${backup_out_name}"
   
   # Backup the sys.inp, sys.out files from last submission
   cp sys.inp ${backup_inp_name}
   cp sys.out ${backup_out_name}
   
   # Make new sys.inp file for submission
   cp sys-1.restart sys.inp
   
   # Load cp2k
   module load cp2k
   
   # Change to the submission directory
   cd $SLURM_SUBMIT_DIR
   
   #Ensure that no libraries are inadvertently using threading
   export OMP_NUM_THREADS=1
   
   # Launch the parallel job
   srun cp2k.popt sys.inp > sys.out
   ```

```shell
cd ~/mapi/nvt-400K
```

   ```shell
   sbatch resub.slurm
   ```

<img src="../nvt-400K/120ps_sbatch.png" alt="120ps_sbatch" style="width:60%;" />

### 130 ps

Change to the working directory and submit the job by the `resub.slurm` script.

```shell
cd mapi/nvt-400K && sbatch resub.slurm
```

Check the job status:

```shell
squeue -u uonlei
```

<img src="../nvt-400K/130ps_sbatch.png" alt="130ps_sbatch" style="width:60%;" />

### 140 ps

Change to the working directory and submit the job by the `resub.slurm` script.

```shell
cd mapi/nvt-400K && sbatch resub.slurm
```

Check the job status:

```shell
squeue -u uonlei
```

<img src="../nvt-400K/140ps_sbatch.png" alt="140ps_sbatch" style="width:60%;" />

### 150 ps

Change to the working directory and submit the job by the `resub.slurm` script.

```shell
cd mapi/nvt-400K && sbatch resub.slurm
```

Check the job status:

```shell
squeue -u uonlei
```

<img src="../nvt-400K/150ps_sbatch.png" alt="150ps_sbatch" style="width:60%;" />

### 160 ps

Change to the working directory and submit the job by the `resub.slurm` script.

```shell
cd mapi/nvt-400K && sbatch resub.slurm
```

Check the job status:

```shell
squeue -u uonlei
```

<img src="../nvt-400K/160ps_sbatch.png" alt="160ps_sbatch" style="width:60%;" />

### 170 ps

Change to the working directory and submit the job by the `resub.slurm` script.

```shell
cd mapi/nvt-400K && sbatch resub.slurm
```

Check the job status:

```shell
squeue -u uonlei
```

<img src="../nvt-400K/170ps_sbatch.png" alt="170ps_sbatch" style="width:60%;" />

### 180 ps

Change to the working directory and submit the job by the `resub.slurm` script.

```shell
cd mapi/nvt-400K && sbatch resub.slurm
```

Check the job status:

```shell
squeue -u uonlei
```

<img src="../nvt-400K/180ps_sbatch.png" alt="180ps_sbatch" style="width:60%;" />

### 190 ps

Change to the working directory and submit the job by the `resub.slurm` script.

```shell
cd ~/mapi/nvt-400K && sbatch resub.slurm
```

Check the job status:

```shell
squeue -u uonlei
```

<img src="../nvt-400K/190ps_sbatch.png" alt="190ps_sbatch" style="width:60%;" />

### 200 ps

Change to the working directory and submit the job by the `resub.slurm` script.

```shell
cd ~/mapi/nvt-400K && sbatch resub.slurm
```

Check the job status:

```shell
squeue -u uonlei
```

<img src="../nvt-400K/190ps_sbatch.png" alt="190ps_sbatch" style="width:60%;" />
