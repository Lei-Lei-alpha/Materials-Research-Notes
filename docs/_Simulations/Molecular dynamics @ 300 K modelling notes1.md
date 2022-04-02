---
layout: post
title:  MAPI + proton AIMD @ 300 K
author: Lei Lei
date:   2022-04-02 14:34:07 +0100
category: Lab Notes
---


This note records the ab initio molecular dynamics (AIMD) simulation job run on the UK tier2 HPC young. 

The scripts are prepared by Sanliang Ling.

---

The job.pbs script modified by Lei is as follow:

```
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
#$ -wd /home/mmm0978/Scratch/cp2k/mapi/nvt-300K

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

The job was submitted at 19 May 2021 by Lei

```
qsub job.pbs
```

The screen shot of job status:

<img src="../nvt-300K/10ps_sub.png" alt="10ps_sub" style="width:60%;" />

The job was submitted three time:

1. job no. 306683:

   <img src="../nvt-300K/10ps_sub1_stat.png" alt="10ps_sub1_stat" style="width:60%;" />

2. job no. 306803

   <img src="../nvt-300K/10ps_sub2_stat_q.png" alt="10ps_sub2_stat_q" style="width:60%;" />

   The job starts to run at 16:26 19 May but ended without calculating anything:

   <img src="../nvt-300K/10ps_sub2_stat_r.png" alt="10ps_sub2_stat_r" style="width:60%;" />

3. job no. 306867

   <img src="../nvt-300K/10ps_sub3_stat_qw.png" alt="10ps_sub3_stat_qw" style="width:60%;" />

   The job starts to  run at 16:26 19 May:

   <img src="../nvt-300K/10ps_sub3_stat_r.png" alt="10ps_sub3_stat_r" style="width:60%;" />

   Then the job status changed to Rq which means pre-job check on a node failed and the job was put back in the queue 19 May 2021:

   <img src="../nvt-300K/10ps_Rq.png" alt="10ps_Rq" style="width:60%;" />
   
   The job was resubmitted at 27 May:
   
   <img src="../nvt-300K/10ps_2705_sub.png" alt="10ps_2705_sub" style="width:60%;" />
   
   It runs this time:
   
   <img src="../nvt-300K/10ps_running.png" alt="10ps_running" style="width:60%;" />

### 20 ps

This job adds another 20000 AIMD steps (10 ps) to the existing trajectory. 

1. Back up sys.inp & sys.out files:

   ```
   cp sys.inp sys_22000_2805.inp && sys.out sys-22000_2805.out
   ```

   <img src="../nvt-300K/20ps_backup.png" alt="20ps_backup" style="width:60%;" />

   <img src="../nvt-300K/20ps_backup_check.png" alt="20ps_backup_check" style="width:60%;" />
   
2. Make new sys.inp file from the sys-1.restart

   ```shell
   cp sys-1.restart sys.inp
   ```

   <img src="../nvt-300K/20ps_make_inp.png" alt="20ps_make_inp" style="width:60%;" />

   Check sys.inp file, should start from 22000:

   ```
   nano sys.inp
   ```

   <img src="../nvt-300K/20ps_check_inp.png" alt="20ps_make_inp" style="width:60%;" />

3. Submit the job again:

   ```
   qsub job.pbs
   ```

   <img src="../nvt-300K/20ps_qsub.png" alt="20ps_qsub" style="width:60%;" />

   Check job status:

   ```
   qstat
   ```

   <img src="../nvt-300K/20ps_qw.png" alt="20ps_qw" style="width:60%;" />

   The job starts to run at 18:43 28 May:
   
   <img src="../nvt-300K/20ps_running.png" alt="20ps_running" style="width:60%;" />

### 30 ps

This job adds another 20000 AIMD steps (10 ps) to the existing trajectory. 

1. Backup sys.inp & sys.out files

```
cp sys.inp sys_42000_2905.inp && cp sys.out sys_42000_2905.out
```

<img src="../nvt-300K/30ps_backup.png" alt="30ps_backup" style="width:60%;" />

<img src="../nvt-300K/30ps_backup_check.png" alt="30ps_backup_check" style="width:60%;" />

2. Make new sys.inp file from sys-1.restart file

```shell
cp sys-1.restart sys.inp
```

<img src="../nvt-300K/30ps_make_inp.png" alt="30ps_make_inp" style="width:60%;" />

Check the new sys.inp file, the STEP_START_VAL should be 42000:

```Shell
nano sys.inp
```

<img src="../nvt-300K/30ps_check_inp.png" alt="30ps_check_inp" style="width:60%;" />

3. Submit the job again by the command:

```Shell
qsub job.pbs
```

<img src="../nvt-300K/30ps_qsub.png" alt="30ps_qsub" style="width:60%;" />

Check job status:

```
qstat
```

<img src="../nvt-300K/30ps_qw.png" alt="30ps_qw" style="width:60%;" />

The job starts to run at 18: 12 15 May.

<img src="../nvt-300K/30ps_running.png" alt="30ps_running" style="width:60%;" />

### 40 ps

Record of AIMD simulation at 300 K, 40 ps.

1. Backup sys.inp & sys.out files.

   Open the directory:

   ```
   cd Scratch/cp2k/mapi/nvt-300K
   ```

   Backup files:

   ```
   cp sys.inp sys_62000_3005.inp && cp sys.out sys_62000_3005.out
   ```
   <img src="../nvt-300K/40ps_backup.png" alt="40ps_backup" style="width:60%;" />

   <img src="../nvt-300K/40ps_backup_check.png" alt="40ps_backup_check" style="width:60%;" />

2. Make new sys.inp file from sys-1.restart file

   ```
   cp sys-1.restart sys.inp
   ```

   <img src="../nvt-300K/40ps_make_inp.png" alt="40ps_make_inp" style="width:60%;" />

3. Check the new sys.inp file, the STEP_START_VAL should be 62000:

   ```
   nano sys.inp
   ```

   <img src="../nvt-300K/40ps_check_inp.png" alt="40ps_check_inp" style="width:60%;" />

4. Submit the job again by the command:

   ```
   qsub job.pbs
   ```

   <img src="../nvt-300K/40ps_qsub.png" alt="40ps_qsub" style="width:60%;" />

   Check job status:

   ```
   qstat
   ```

   <img src="../nvt-300K/40ps_qw.png" alt="40ps_qw" style="width:60%;" />

   The job starts to run at 22: 40 30 May.

   <img src="../nvt-300K/40ps_running.png" alt="40ps_running" style="width:60%;" />

   

### 50 ps

Record of AIMD simulation at 300 K, 50 ps.

1. Backup sys.inp & sys.out files.

   Open the directory:

   ```
   cd Scratch/cp2k/mapi/nvt-300K
   ```

   Backup files:

   ```
   cp sys.inp sys_82000_2806.inp && cp sys.out sys_82000_2806.out
   ```

   <img src="../nvt-300K/50ps_backup.png" alt="50ps_backup" style="width:60%;" />

   <img src="../nvt-300K/50ps_backup_check.png" alt="50ps_backup_check" style="width:60%;" />

2. Make new sys.inp file from sys-1.restart file

   ```
   cp sys-1.restart sys.inp
   ```

   <img src="../nvt-300K/50ps_make_inp.png" alt="50ps_make_inp" style="width:60%;" />

3. Check the new sys.inp file, the STEP_START_VAL should be 82000:

   ```
   nano sys.inp
   ```

   <img src="../nvt-300K/50ps_check_inp.png" alt="50ps_check_inp" style="width:60%;" />

4. Submit the job again by the command:

   ```
   qsub job.pbs
   ```

   <img src="../nvt-300K/50ps_qsub.png" alt="50ps_qsub" style="width:60%;" />

   Check job status:

   ```
   qstat
   ```

   <img src="../nvt-300K/50ps_qw.png" alt="50ps_qw" style="width:60%;" />

   The job starts to run at 22: 40 30 May.

   <img src="../nvt-300K/50ps_running.png" alt="50ps_running" style="width:60%;" />

The work stopped at 90520 step (45 ps).

### 55 ps

Record of AIMD simulation at 300 K, 50 ps.

1. Backup sys.inp & sys.out files.

   Open the directory:

   ```
   cd Scratch/cp2k/mapi/nvt-300K
   ```

   Backup files:

   ```
   cp sys.inp sys_90500_2806.inp && cp sys.out sys_90500_2806.out
   ```

   <img src="../nvt-300K/55ps_backup.png" alt="50ps_backup" style="width:60%;" />

   <img src="../nvt-300K/55ps_backup_check.png" alt="50ps_backup_check" style="width:60%;" />

2. Make new sys.inp file from sys-1.restart file

   ```
   cp sys-1.restart sys.inp
   ```

   <img src="../nvt-300K/55ps_make_inp.png" alt="55ps_make_inp" style="width:60%;" />

3. Check the new sys.inp file, the STEP_START_VAL was 90520:

   ```
   nano sys.inp
   ```

   <img src="../nvt-300K/55ps_check_inp.png" alt="55ps_check_inp" style="width:60%;" />

4. Submit the job again by the command:

   ```
   qsub job.pbs
   ```

   <img src="../nvt-300K/55ps_qsub.png" alt="55ps_qsub" style="width:60%;" />

   Check job status:

   ```
   qstat
   ```

   <img src="../nvt-300K/55ps_qw.png" alt="55ps_qw" style="width:60%;" />

   The job starts to run at 22: 40 30 May.

   <img src="../nvt-300K/55ps_running.png" alt="55ps_running" style="width:60%;" />

### 65 ps

Record of AIMD simulation at 300 K, 50 ps.

1. Backup sys.inp & sys.out files.

   Open the directory:

   ```
   cd Scratch/cp2k/mapi/nvt-300K
   ```

   Backup files:

   ```
   cp sys.inp sys_11050_1307.inp && cp sys.out sys_11050_1307.out && ls
   ```

   <img src="../nvt-300K/55ps_backup.png" alt="50ps_backup" style="width:60%;" />

   <img src="../nvt-300K/55ps_backup_check.png" alt="50ps_backup_check" style="width:60%;" />

2. Make new sys.inp file from sys-1.restart file

   ```
   cp sys-1.restart sys.inp && nano sys.inp
   ```

   <img src="../nvt-300K/65ps_make_inp.png" alt="55ps_make_inp" style="width:60%;" />

3. Check the new sys.inp file, the STEP_START_VAL was 110520:

   <img src="../nvt-300K/65ps_check_inp.png" alt="55ps_check_inp" style="width:60%;" />

4. Submit the job again by the command:

   ```
   qsub job.pbs
   ```

   <img src="../nvt-300K/55ps_qsub.png" alt="55ps_qsub" style="width:60%;" />

   Check job status, the job starts to run at 17:36 13 July:

   ```
   qstat
   ```

   <img src="../nvt-300K/65ps_running.png" alt="55ps_qw" style="width:60%;" />

### 80 ps

   Record of AIMD simulation at 300 K, 80 ps.

   1. Backup sys.inp & sys.out files.
   
      Open the directory:
   
      ```
      cd Scratch/cp2k/mapi/nvt-300K
      ```
   
      Backup files:
   
      ```
      cp sys.inp sys_13200_1407.inp && cp sys.out sys_13200_1407.out && ls
      ```
   
      <img src="../nvt-300K/80ps_backup.png" alt="50ps_backup" style="width:60%;" />
   
      <img src="../nvt-300K/80ps_backup_check.png" alt="80ps_backup_check" style="width:60%;" />
   
   2. Make new sys.inp file from sys-1.restart file
   
      ```
      cp sys-1.restart sys.inp && nano sys.inp
      ```
   
      <img src="../nvt-300K/80ps_make_inp.png" alt="80ps_make_inp" style="width:60%;" />
   
   3. Check the new sys.inp file, the STEP_START_VAL was 132000:
   
      <img src="../nvt-300K/80ps_check_inp.png" alt="80ps_check_inp" style="width:60%;" />
   
   4. Submit the job again by the command:
   
      ```
      qsub job.pbs
      ```
   
      <img src="../nvt-300K/80ps_qsub.png" alt="80ps_qsub" style="width:60%;" />
   
      Check job status, the job starts to run at 21:11 14 July:
   
      ```
      qstat
      ```
   
      <img src="../nvt-300K/80ps_qw.png" alt="80ps_qw" style="width:60%;" />
   
      <img src="../nvt-300K/80ps_running.png" alt="80ps_running" style="width:60%;" />

### 90 ps

Record of AIMD simulation at 300 K, 90 ps.

   1. Backup sys.inp & sys.out files.

      Open the directory:

      ```
      cd Scratch/cp2k/mapi/nvt-300K
      ```

      Backup files:

      ```
      cp sys.inp sys_162000.inp && cp sys.out sys_162000_1607.out && ls
      ```

      <img src="../nvt-300K/90ps_backup.png" alt="90ps_backup" style="width:60%;" />

      <img src="../nvt-300K/90ps_backup_check.png" alt="90ps_backup_check" style="width:60%;" />

   2. Make new sys.inp file from sys-1.restart file

      ```
      cp sys-1.restart sys.inp && nano sys.inp
      ```

      <img src="../nvt-300K/90ps_make_inp.png" alt="90ps_make_inp" style="width:60%;" />

   3. Check the new sys.inp file, the STEP_START_VAL was 162000:

      <img src="../nvt-300K/90ps_check_inp.png" alt="90ps_check_inp" style="width:60%;" />

   4. Submit the job again by the command:

      ```
      qsub job.pbs
      ```

      <img src="../nvt-300K/90ps_qsub.png" alt="90ps_qsub" style="width:60%;" />

      Check job status, the job starts to run at 11:02 16 July:

      ```
      qstat
      ```

      <img src="../nvt-300K/90ps_qw.png" alt="90ps_qw" style="width:60%;" />

      <img src="../nvt-300K/90ps_running.png" alt="90ps_running" style="width:60%;" />
### 100 ps
Record of AIMD simulation at 300 K, 100 ps.

   1. Backup sys.inp & sys.out files.

      Open the directory:

      ```
      cd Scratch/cp2k/mapi/nvt-300K
      ```

      Backup files:

      ```
      cp sys.inp sys_182000_1707.inp && cp sys.out sys_182000_1707.out && ls
      ```

      <img src="../nvt-300K/100ps_backup.png" alt="100ps_backup" style="width:60%;" />

      <img src="../nvt-300K/100ps_backup_check.png" alt="100ps_backup_check" style="width:60%;" />

   2. Make new sys.inp file from sys-1.restart file

      ```
      cp sys-1.restart sys.inp && nano sys.inp
      ```

      <img src="../nvt-300K/100ps_make_inp.png" alt="100ps_make_inp" style="width:60%;" />

   3. Check the new sys.inp file, the STEP_START_VAL was 182000:

      <img src="../nvt-300K/100ps_check_inp.png" alt="100ps_check_inp" style="width:60%;" />

   4. Submit the job again by the command:

      ```
      qsub job.pbs
      ```

      <img src="../nvt-300K/100ps_qsub.png" alt="100ps_qsub" style="width:60%;" />

      Check job status, the job starts to run at 11:02 16 July:

      ```
      qstat
      ```

      <img src="../nvt-300K/100ps_qw.png" alt="100ps_qw" style="width:60%;" />

      <img src="../nvt-300K/100ps_running.png" alt="100ps_running" style="width:60%;" />
      
### 110 ps

~~~shell
     The 60 ps job was run on Cirrus. The submission script used is `job.sh`:
  
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
~~~

   1. Open the directory:

      ```
      cd mapi/nvt-300K
      ```

2. Convert the `job.slurm` file to unix format.

   ```
   dos2unix job.slurm
   ```

   Backup files:

   ```
   cp sys.inp sys_202000_1208.inp && cp sys.out sys_202000_1208.out
   ```

<img src="../nvt-300K/110ps_backup.png" alt="110ps_backup" style="width:60%;" />

<img src="../nvt-300K/110ps_backup_check.png" alt="110ps_backup_check" style="width:60%;" />

3. Make new sys.inp file from sys-1.restart file

   ```
   cp sys-1.restart sys.inp
   ```

<img src="../nvt-300K/110ps_check_inp.png" alt="110ps_backup_check" style="width:60%;" />

4. Submit the job:

   ```shell
   sbatch job.slurm && squeue -u uonlei
   ```

<img src="../nvt-300K/110ps_sbatch&pd.png" alt="110ps_sbatch&pd" style="width:60%;" />



### 120 ps

1. Backup files:

   ```shell
   cd mapi/nvt-300K
   ```

   <img src="../nvt-300K/120ps_cd2dir.png" alt="120ps_cd2dir" style="width:60%;" />

   ```shell
   cp sys.inp sys_222000_1408.inp && cp sys.out sys_222000_1408.out
   ```

   <img src="../nvt-300K/120ps_backup.png" alt="120ps_backup" style="width:60%;" />

   <img src="../nvt-300K/120ps_backup_check.png" alt="120ps_backup_check" style="width:60%;" />

2. Make new sys.inp file from sys-1.restart file

   ```shell
   cp sys-1.restart sys.inp && nano sys.inp
   ```

   <img src="../nvt-300K/120ps_make_inp.png" alt="120ps_make_inp" style="width:60%;" />

Check the new `sys.inp` file, the `STEP_START_VAL` should be 222000:

<img src="../nvt-300K/120ps_check_inp.png" alt="120ps_check_inp" style="width:60%;" />

3. Submit the job:

   ```shell
   sbatch job.slurm && squeue -u uonlei
   ```

   <img src="../nvt-300K/120ps_sbatch.png" alt="120ps_sbatch" style="width:60%;" />

   Check the job status:

   ```shell
   squeue -u uonlei
   ```

   <img src="../nvt-300K/120ps_running.png" alt="120ps_running" style="width:60%;" />

### 130 ps

1. Backup files:

```shell
cd mapi/nvt-300K
```

```
cp sys.inp sys_242000_1508.inp && cp sys.out sys_242000_1508.out && ls
```

<img src="../nvt-300K/130ps_backup.png" alt="130ps_backup" style="width:60%;" />

<img src="../nvt-300K/130ps_backup_check.png" alt="130ps_backup_check" style="width:60%;" />

2. Make new `sys.inp` file from `sys-1.restart `file

```
cp sys-1.restart sys.inp && nano sys.inp
```

<img src="../nvt-300K/130ps_make_inp.png" alt="130ps_make_inp" style="width:60%;" />

Check the new `sys.inp` file, the `STEP_START_VAL` should be 242000:

<img src="../nvt-300K/130ps_check_inp.png" alt="130ps_check_inp" style="width:60%;" />

3. Submit the job:

```shell
sbatch job.slurm && squeue -u uonlei
```

<img src="../nvt-300K/130ps_sbatch.png" alt="130ps_sbatch" style="width:60%;" />

Check the job status:

```shell
squeue -u uonlei
```

<img src="../nvt-300K/130ps_running.png" alt="130ps_running" style="width:60%;" />

### 140 ps

1. Backup files:

```shell
cd mapi/nvt-300K
```

```
cp sys.inp sys_262000_1608.inp && cp sys.out sys_262000_1608.out && ls
```

<img src="../nvt-300K/140ps_backup.png" alt="140ps_backup" style="width:60%;" />

<img src="../nvt-300K/140ps_backup_check.png" alt="140ps_backup_check" style="width:60%;" />

2. Make new `sys.inp` file from `sys-1.restart `file

```
cp sys-1.restart sys.inp && nano sys.inp
```

<img src="../nvt-300K/140ps_make_inp.png" alt="140ps_make_inp" style="width:60%;" />

Check the new `sys.inp` file, the `STEP_START_VAL` should be ==262000==:

<img src="../nvt-300K/140ps_check_inp.png" alt="140ps_check_inp" style="width:60%;" />

3. Submit the job:

```shell
sbatch job.slurm && squeue -u uonlei
```

<img src="../nvt-300K/140ps_sbatch.png" alt="140ps_sbatch" style="width:60%;" />

Check the job status:

```shell
squeue -u uonlei
```

<img src="../nvt-300K/140ps_running.png" alt="140ps_running" style="width:60%;" />

### 150 ps

1. Backup files:

```shell
cd mapi/nvt-300K
```

```
cp sys.inp sys_282000_1808.inp && cp sys.out sys_282000_1808.out && ls
```

<img src="../nvt-300K/150ps_backup.png" alt="150ps_backup" style="width:60%;" />

<img src="../nvt-300K/150ps_backup_check.png" alt="150ps_backup_check" style="width:60%;" />

2. Make new `sys.inp` file from `sys-1.restart `file

```
cp sys-1.restart sys.inp && nano sys.inp
```

<img src="../nvt-300K/150ps_make_inp.png" alt="150ps_make_inp" style="width:60%;" />

Check the new `sys.inp` file, the `STEP_START_VAL` should be ==282000==:

<img src="../nvt-300K/150ps_check_inp.png" alt="150ps_check_inp" style="width:60%;" />

3. Submit the job:

```shell
sbatch job.slurm && squeue -u uonlei
```

<img src="../nvt-300K/150ps_sbatch.png" alt="140ps_sbatch" style="width:60%;" />

Check the job status:

```shell
squeue -u uonlei
```

<img src="../nvt-300K/150ps_running.png" alt="140ps_running" style="width:60%;" />

### 160 ps

1. Backup files:

```shell
cd mapi/nvt-300K
```

```
cp sys.inp sys_302000_1908.inp && cp sys.out sys_302000_1908.out && ls
```

<img src="../nvt-300K/160ps_backup.png" alt="160ps_backup" style="width:60%;" />

<img src="../nvt-300K/160ps_backup_check.png" alt="160ps_backup_check" style="width:60%;" />

2. Make new `sys.inp` file from `sys-1.restart `file

```
cp sys-1.restart sys.inp && nano sys.inp
```

<img src="../nvt-300K/160ps_make_inp.png" alt="160ps_make_inp" style="width:60%;" />

Check the new `sys.inp` file, the `STEP_START_VAL` should be ==302000==:

<img src="../nvt-300K/160ps_check_inp.png" alt="160ps_check_inp" style="width:60%;" />

3. Submit the job:

```shell
sbatch job.slurm && squeue -u uonlei
```

<img src="../nvt-300K/160ps_sbatch.png" alt="160ps_sbatch" style="width:60%;" />

Check the job status:

```shell
squeue -u uonlei
```

<img src="../nvt-300K/160ps_running.png" alt="160ps_running" style="width:60%;" />

### 170 ps

Change to the working directory.

```shell
cd mapi/nvt-300K
```
Use the following script to back up files and resubmit the job.

```shell
#!/bin/bash

# Slurm job options (name, compute nodes, job time)
#SBATCH --job-name=CP2K_200K
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

The above script was converted to unix format:

```shell
dos2unix resub.slurm
```

<img src="../nvt-300K/170ps_convert.png" alt="170ps_convert" style="width:60%;" />

Submit the job by the `resub.slurm` script:

```shel
sbatch resub.slurm
```

<img src="../nvt-300K/170ps_sbatch.png" alt="170ps_sbatch" style="width:60%;" />



Check the new `sys.inp` file, the `STEP_START_VAL` should be ==322000==:

```
nano sys.inp
```

<img src="../nvt-300K/170ps_check_inp.png" alt="170ps_check_inp" style="width:60%;" />

Check the job status:

```shell
squeue -u uonlei
```

<img src="../nvt-300K/170ps_running.png" alt="170ps_running" style="width:60%;" />

### 180 ps

Change to the working directory.

```shell
cd mapi/nvt-300K
```

Submit the job by the `resub.slurm` script:

```shell
sbatch resub.slurm
```

<img src="../nvt-300K/180ps_sbatch.png" alt="180ps_sbatch" style="width:60%;" />

Check the job status:

```shell
squeue -u uonlei
```

<img src="../nvt-300K/180ps_running.png" alt="180ps_running" style="width:60%;" />

### 190 ps

Change to the working directory.

```shell
cd mapi/nvt-300K
```

Submit the job by the `resub.slurm` script:

```shell
sbatch resub.slurm
```

<img src="../nvt-300K/190ps_sbatch.png" alt="190ps_sbatch" style="width:60%;" />

Check the job status:

```shell
squeue -u uonlei
```

<img src="../nvt-300K/190ps_pd.png" alt="190ps_pd" style="width:60%;" />


### 200 ps

Change to the working directory and submit the job by the `resub.slurm` script.

```shell
cd mapi/nvt-300K && sbatch resub.slurm
```

<img src="../nvt-300K/200ps_sbatch.png" alt="200ps_sbatch" style="width:60%;" />

Check the job status:

```shell
squeue -u uonlei
```

<img src="../nvt-300K/200ps_running.png" alt="200ps_running" style="width:60%;" />

### 210 ps

Change to the working directory and submit the job by the `resub.slurm` script.

```shell
cd mapi/nvt-300K && sbatch resub.slurm
```

Check the job status:

```shell
squeue -u uonlei
```

<img src="../nvt-300K/210ps_sbatch.png" alt="200ps_sbatch" style="width:60%;" />

The job did not finish within the time limit and stopped. The job did not run correctly when submitted again due to the wavefunction file   from last unfinished job is not complete. 

Firstly, identify the problem. The situation can happen due to the longer time taken by the job (the SCF take longer to converge due to the setting) or due to a problem of HPC (e.g. the hard disk problem).

Firstly, check the output file, go to the end of the file with the option `+G`:

```
less +G sys-1.ener
```

<img src="../nvt-300K/210ps_check_ener_command.png" alt="210ps_check_ener_command" style="width:60%;" />

<img src="../nvt-300K/210ps_check_ener.png" alt="210ps_check_ener" style="width:60%;" />

Check the output file to see how many steps did the job taken to converge:

```
grep "outer SCF l" sys.out
```

<img src="../nvt-300K/210ps_check_scf.png" alt="210ps_check_scf" style="width:60%;" />

At last check the movie to see if all the steps are normal or not.

If the problem is confirmed to be the HPC. Use the backup wavefunction `sys-RESTART.wfn.bak-1` to continue the calculation:

```
cp sys-RESTART.wfn.bak-1 sys-RESTART.wfn
```

<img src="../nvt-300K/210ps_make_wavefunc.png" alt="210ps_make_wavefunc" style="width:60%;" />

Submit the job again and add the unfinished steps in the 220 ps simulation.

### 220 ps

1. Backup files:

```
cp sys.inp sys_421000_0909.inp && cp sys.out sys_421000_0909.out && ls
```

<img src="../nvt-300K/210ps_resub_backup.png" alt="210ps_resub_backup" style="width:60%;" />

<img src="../nvt-300K/210ps_resub_backup_check.png" alt="210ps_resub_backup_check" style="width:60%;" />

2. Make new `sys.inp` file from `sys-1.restart `file

```
cp sys-1.restart sys.inp && nano sys.inp
```

<img src="../nvt-300K/210ps_resub_make_inp.png" alt="210ps_resub_make_inp" style="width:60%;" />

Check the new `sys.inp` file, change the steps, so that the simulation ends at desired steps:

<img src="../nvt-300K/210ps_resub_check_inp.png" alt="210ps_resub_check_inp" style="width:60%;" />

3. Submit the job:

```shell
sbatch job.slurm && squeue -u uonlei
```

<img src="../nvt-300K/220ps_sbatch.png" alt="160ps_sbatch" style="width:60%;" />

### 230 ps

1. Backup files:

```
cp sys.inp sys_442000_1109.inp && cp sys.out sys_442000_1109.out && ls
```

<img src="../nvt-300K/230ps_backup.png" alt="230ps_backup" style="width:60%;" />

<img src="../nvt-300K/230ps_backup_check.png" alt="230ps_backup_check" style="width:60%;" />

2. Make new `sys.inp` file from `sys-1.restart `file

```
cp sys-1.restart sys.inp && nano sys.inp
```

<img src="../nvt-300K/230ps_make_inp.png" alt="230ps_make_inp" style="width:60%;" />

Check the new `sys.inp` file, change the steps, so that the simulation ends at desired steps:

<img src="../nvt-300K/230ps_check_inp.png" alt="230ps_check_inp" style="width:60%;" />

3. Submit the job:

```shell
sbatch job.slurm && squeue -u uonlei
```

<img src="../nvt-300K/230ps_sbatch.png" alt="230ps_sbatch" style="width:60%;" />

### 240 ps

Change to the working directory and submit the job by the `resub.slurm` script.

```shell
cd mapi/nvt-300K && sbatch resub.slurm
```

Check the job status:

```shell
squeue -u uonlei
```

<img src="../nvt-300K/240ps_sbatch.png" alt="240ps_sbatch" style="width:60%;" />

### 250 ps

Change to the working directory and submit the job by the `resub.slurm` script.

```shell
cd ~/mapi/nvt-300K && sbatch resub.slurm
```

Check the job status:

```shell
squeue -u uonlei
```

<img src="../nvt-300K/240ps_sbatch.png" alt="240ps_sbatch" style="width:60%;" />

### 260 ps

1. Backup files:

```
cp sys.inp sys_496000_1609.inp && cp sys.out sys_496000_1609.out && ls
```

<img src="../nvt-300K/260ps_backup.png" alt="260ps_backup" style="width:60%;" />

<img src="../nvt-300K/260ps_backup_check.png" alt="260ps_backup_check" style="width:60%;" />

2. Make new `sys.inp` file from `sys-1.restart `file

```
cp sys-1.restart sys.inp && nano sys.inp
```

<img src="../nvt-300K/260ps_make_inp.png" alt="260ps_make_inp" style="width:60%;" />

Check the new `sys.inp` file, change the steps, so that the simulation ends at desired steps:

<img src="../nvt-300K/260ps_check_inp.png" alt="260ps_check_inp" style="width:60%;" />

3. Submit the job:

```shell
sbatch job.slurm && squeue -u uonlei
```

### 270 ps

1. Backup files:

```
cp sys.inp sys_522000_1809.inp && cp sys.out sys_522000_1809.out && ls
```

<img src="../nvt-300K/270ps_backup.png" alt="270ps_backup" style="width:60%;" />

<img src="../nvt-300K/270ps_backup_check.png" alt="270ps_backup_check" style="width:60%;" />

2. Make new `sys.inp` file from `sys-1.restart `file

```
cp sys-1.restart sys.inp && nano sys.inp
```

<img src="../nvt-300K/270ps_make_inp.png" alt="270ps_make_inp" style="width:60%;" />

Check the new `sys.inp` file, change the steps, so that the simulation ends at desired steps:

<img src="../nvt-300K/270ps_check_inp.png" alt="270ps_check_inp" style="width:60%;" />

3. Submit the job:

```shell
sbatch job.slurm && squeue -u uonlei
```

<img src="../nvt-300K/270ps_sbatch.png" alt="270ps_sbatch" style="width:60%;" />

### 280 ps

Change to the working directory and submit the job by the `resub.slurm` script.

```shell
cd ~/mapi/nvt-300K && sbatch resub.slurm
```

Check the job status:

```shell
squeue -u uonlei
```

<img src="../nvt-300K/280ps_sbatch.png" alt="280ps_sbatch" style="width:60%;" />

### 290 ps

Change to the working directory and submit the job by the `resub.slurm` script.

```shell
cd ~/mapi/nvt-300K && sbatch resub.slurm
```

Check the job status:

```shell
squeue -u uonlei
```

<img src="../nvt-300K/290ps_sbatch.png" alt="290ps_sbatch" style="width:60%;" />

### 300 ps

Change to the working directory and submit the job by the `resub.slurm` script.

```shell
cd ~/mapi/nvt-300K && sbatch resub.slurm
```

Check the job status:

```shell
squeue -u uonlei
```

<img src="../nvt-300K/300ps_sbatch.png" alt="300ps_sbatch" style="width:60%;" />

### 310 ps

Change to the working directory and submit the job by the `resub.slurm` script.

```shell
cd ~/mapi/nvt-300K && sbatch resub.slurm
```

Check the job status:

```shell
squeue -u uonlei
```

<img src="../nvt-300K/310ps_sbatch.png" alt="310ps_sbatch" style="width:60%;" />

### 320 ps

Change to the working directory and submit the job by the `resub.slurm` script.

```shell
cd ~/mapi/nvt-300K && sbatch resub.slurm
```

Check the job status:

```shell
squeue -u uonlei
```

<img src="../nvt-300K/320ps_sbatch.png" alt="320ps_sbatch" style="width:60%;" />

### 330 ps

Change to the working directory and submit the job by the `resub.slurm` script.

```shell
cd ~/mapi/nvt-300K && sbatch resub.slurm
```

Check the job status:

```shell
squeue -u uonlei
```

<img src="../nvt-300K/330ps_sbatch.png" alt="330ps_sbatch" style="width:60%;" />

### 340 ps

Change to the working directory and submit the job by the `resub.slurm` script.

```shell
cd ~/mapi/nvt-300K && sbatch resub.slurm
```

Check the job status:

```shell
squeue -u uonlei
```

<img src="../nvt-300K/340ps_sbatch.png" alt="340ps_sbatch" style="width:60%;" />

### 350 ps

Change to the working directory and submit the job by the `resub.slurm` script.

```shell
cd ~/mapi/nvt-300K && sbatch resub.slurm
```

Check the job status:

```shell
squeue -u uonlei
```

<img src="../nvt-300K/350ps_sbatch.png" alt="350ps_sbatch" style="width:60%;" />

<img src="../nvt-300K/350ps_squeue.png" alt="350ps_squeue" style="width:60%;" />

### 360 ps

Change to the working directory and submit the job by the `resub.slurm` script.

```shell
cd ~/mapi/nvt-300K && sbatch resub.slurm
```

Check the job status:

```shell
squeue -u uonlei
```

<img src="../nvt-300K/360ps_sbatch.png" alt="360ps_sbatch" style="width:60%;" />

### 370 ps

Change to the working directory and submit the job by the `resub.slurm` script.

```shell
cd ~/mapi/nvt-300K && sbatch resub.slurm
```

Check the job status:

```shell
squeue -u uonlei
```

<img src="../nvt-300K/370ps_sbatch.png" alt="370ps_sbatch" style="width:60%;" />

### 380 ps

Change to the working directory and submit the job by the `resub.slurm` script.

```shell
cd ~/mapi/nvt-300K && sbatch resub.slurm
```

Check the job status:

```shell
squeue -u uonlei
```

<img src="../nvt-300K/380ps_sbatch.png" alt="380ps_sbatch" style="width:60%;" />

### 390 ps

Change to the working directory and submit the job by the `resub.slurm` script.

```shell
cd ~/mapi/nvt-300K && sbatch resub.slurm
```

Check the job status:

```shell
squeue -u uonlei
```

<img src="../nvt-300K/390ps_sbatch.png" alt="390ps_sbatch" style="width:60%;" />

### 400 ps

Change to the working directory and submit the job by the `resub.slurm` script.

```shell
cd ~/mapi/nvt-300K && sbatch resub.slurm
```

Check the job status:

```shell
squeue -u uonlei
```

<img src="../nvt-300K/400ps_sbatch.png" alt="400ps_sbatch" style="width:60%;" />
