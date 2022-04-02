---
layout: post
title:  MAPI + proton AIMD @ 200 K
author: Lei Lei
date:   2022-04-02 14:34:07 +0100
category: Lab Notes
---

This note records the ab initio molecular dynamics (AIMD) simulation job run on the UK tier2 HPC young. 

The scripts are prepared by Sanliang Ling.


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
#$ -wd /home/mmm0978/Scratch/cp2k/mapi/nvt-200K

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

<img src="../nvt-200K/10ps_stat_qw.png" alt="10ps_stat_qw" style="width:60%;" />

The job starts to  run at 17:33 19 May:

<img src="../nvt-200K/10ps_stat_r.png" alt="10ps_stat_r" style="width:60%;" />

The job was done at …


### 20 ps

This job adds another 20000 AIMD steps (10 ps) to the existing trajectory. 

1. Back up sys.inp & sys.out files:

   ```
   cp sys.inp sys.0521.inp && cp sys.out sys.0514.out
   ```

   <img src="../nvt-200K/20ps_backup_files.png" alt="20ps_backup_files" style="width:60%;" />

   
   
2. Make new `sys.inp` file from the sys-1.restart

   ```shell
   cp sys-1.restart sys.inp
   ```

   <img src="../nvt-200K/20ps_make_inp.png" alt="20ps_make_inp" style="width:60%;" />

3. Check the new `sys.inp` file, the STEP_START_VAL should be 22000

   ```
   nano sys.inp
   ```

   <img src="../nvt-200K/20ps_inp_check.png" alt="20ps_inp_check" style="width:60%;" />

4. Submit the job again:

   ```
   qsub job.pbs
   ```

   <img src="../nvt-200K/20ps_qsub.png" alt="20ps_qsub" style="width:60%;" />

   Check job status:

   ```
   qstat
   ```

   <img src="../nvt-200K/20ps_qw.png" alt="20ps_qw" style="width:60%;" />

   Job job was done at …:


### 30 ps

   Budgets before simulation:

   <img src="../nvt-200K/30ps_budgets.png" alt="30ps_budgets" style="width:60%;" />

   1. Backup sys.inp & sys.out files

      ```
      cd Scratch/cp2k/mapi/nvt-200K
      ```

      ```
      cp sys.inp sys_42000_2605.inp && cp sys.out sys_42000_2605.out
      ```

      <img src="../nvt-200K/30ps_backup.png" alt="30ps_backup" style="width:60%;" />

      <img src="../nvt-200K/30ps_backup_check.png" alt="30ps_backup_check" style="width:60%;" />

   2. Make new sys.inp file from sys-1.restart file

      ```shell
      cp sys-1.restart sys.inp
      ```

      <img src="../nvt-200K/30ps_make_inp.png" alt="newINP_1505" style="width:60%;" />

      Check the new sys.inp file, the STEP_START_VAL should be 42000:

      ```Shell
      nano sys.inp
      ```

      <img src="../nvt-200K/30ps_inp_check.png" alt="30ps_inp_check" style="width:60%;" />

   3. Submit the job again by the command:

      ```Shell
      qsub job.pbs
      ```

      <img src="../nvt-200K/30ps_qsub.png" alt="30ps_qsub" style="width:60%;" />

      Check job status:

      ```
      qstat
      ```

      <img src="../nvt-200K/30ps_qw.png" alt="30ps_qw" style="width:60%;" />

      The job starts to run at 18: 12 15 May.

      <img src="../nvt-400K/Screenshot 2021-05-15 181428_qstat.png" alt="Screenshot 2021-05-15 181428_qstat" style="width:60%;" />

### 40 ps

   Budgets before simulation:

   1. Backup sys.inp & sys.out files

      Open the directory:

      ```
      cd Scratch/cp2k/mapi/nvt-200K
      ```

      Backup files:

      ```
      cp sys.inp sys_62000_2705.inp && cp sys.out sys_62000_2705.out
      ```
      <img src="../nvt-200K/40ps_backup.png" alt="40ps_backup" style="width:60%;" />

      <img src="../nvt-200K/40ps_backup_check.png" alt="40ps_backup_check" style="width:60%;" />

   2. Make new sys.inp file from sys-1.restart file

      ```
      cp sys-1.restart sys.inp
      ```

      <img src="../nvt-200K/40ps_make_inp.png" alt="40ps_make_inp" style="width:60%;" />

   3. Check the new sys.inp file, the STEP_START_VAL should be 62000:

      ```
      nano sys.inp
      ```

      <img src="../nvt-200K/40ps_inp_check.png" alt="40ps_inp_check" style="width:60%;" />

   4. Submit the job again by the command:

      ```
      qsub job.pbs
      ```

      <img src="../nvt-200K/40ps_qsub.png" alt="40ps_qsub" style="width:60%;" />

      Check job status:

      ```
      qstat
      ```

      <img src="../nvt-200K/40ps_qw.png" alt="40ps_qw" style="width:60%;" />

      The job starts to run at 11: 42 28 May.

      <img src="../nvt-400K/40_ps_running.png" alt="40_ps_running" style="width:60%;" />

### 50 ps calculation

   50 ps AIMD simulation record:

   1. Backup sys.inp & sys.out files

      Open the directory:

      ```
      cd Scratch/cp2k/mapi/nvt-200K
      ```

      Backup files:

      ```
      cp sys.inp sys_82000_2805.inp && cp sys.out sys_82000_2805.out
      ```

      <img src="../nvt-200K/50ps_backup.png" alt="50ps_backup" style="width:60%;" />

      <img src="../nvt-200K/50ps_backup_check.png" alt="50ps_backup_check" style="width:60%;" />

   2. Make new sys.inp file from sys-1.restart file

      ```
      cp sys-1.restart sys.inp
      ```

      <img src="../nvt-200K/50ps_make_inp.png" alt="50ps_make_inp" style="width:60%;" />

   3. Check the new sys.inp file, the STEP_START_VAL should be 82000:

      ```
      nano sys.inp
      ```

      <img src="../nvt-200K/50ps_inp_check.png" alt="50ps_inp_check" style="width:60%;" />

   4. Submit the job again by the command:

      ```
      qsub job.pbs
      ```

      <img src="../nvt-200K/50ps_qsub.png" alt="50ps_qsub" style="width:60%;" />

      Check job status:

      ```
      qstat
      ```

      <img src="../nvt-200K/50ps_qw.png" alt="50ps_qw" style="width:60%;" />

      The job starts to run at 15: 22 28 May.

      <img src="../nvt-200K/50ps_running.png" alt="50ps_running" style="width:60%;" />

      The above submission didn’t run correctly. The job was submitted again at 18:03 28 May:

      <img src="../nvt-200K/50ps_qsub2.png" alt="50ps_qsub2" style="width:60%;" />

      The job start to run at 18:19 28 May:

      <img src="../nvt-200K/50ps_running2.png" alt="50ps_running2" style="width:60%;" />

### 60 ps

60 ps AIMD simulation record

1. Backup sys.inp & sys.out files

Open the directory:

```
cd Scratch/cp2k/mapi/nvt-200K
```

Backup files:

```
cp sys.inp sys_102000_2905.inp && cp sys.out sys_102000_2905.out
```

<img src="../nvt-200K/60ps_backup.png" alt="60ps_backup" style="width:60%;" />

<img src="../nvt-200K/60ps_backup_check.png" alt="60ps_backup_check" style="width:60%;" />

2. Make new sys.inp file from sys-1.restart file

```
cp sys-1.restart sys.inp
```

<img src="../nvt-200K/60ps_make_inp.png" alt="60ps_make_inp" style="width:60%;" />

3. Check the new sys.inp file, the STEP_START_VAL should be 102000:

```
nano sys.inp
```

<img src="../nvt-200K/60ps_inp_check.png" alt="60ps_inp_check" style="width:60%;" />

4. Submit the job again by the command:

```
qsub job.pbs
```

<img src="../nvt-200K/60ps_qsub.png" alt="60ps_qsub" style="width:60%;" />

Check job status:

```
qstat
```

<img src="../nvt-200K/60ps_qw.png" alt="60ps_qw" style="width:60%;" />

The job starts to run at 20:24 29 May:

<img src="../nvt-200K/60ps_running.png" alt="60ps_running" style="width:60%;" />

### 70 ps

The AIMD simulation record of 70 ps at 200 K.

1. Backup sys.inp & sys.out files

Open the directory:

```
cd Scratch/cp2k/mapi/nvt-200K
```

Backup files:

```
cp sys.inp sys_122000_3005.inp && cp sys.out sys_122000_3005.out
```

<img src="../nvt-200K/70ps_backup.png" alt="70ps_backup.png" style="width:60%;" />

<img src="../nvt-200K/70ps_backup_check.png" alt="70ps_backup_check" style="width:60%;" />

2. Make new sys.inp file from sys-1.restart file

```
cp sys-1.restart sys.inp
```

<img src="../nvt-200K/70ps_make_inp.png" alt="70ps_make_inp" style="width:60%;" />

3. Check the new sys.inp file, the STEP_START_VAL should be 122000:

```
nano sys.inp
```

<img src="../nvt-200K/70ps_check_inp.png" alt="70ps_check_inp" style="width:60%;" />

4. Submit the job again by the command:

```
qsub job.pbs
```

<img src="../nvt-200K/70ps_qsub.png" alt="70ps_qsub" style="width:60%;" />

Check job status:

```
qstat
```

<img src="../nvt-200K/70ps_running.png" alt="70ps_running" style="width:60%;" />

### 80 ps

The AIMD simulation record of 80 ps at 200 K.

1. Backup sys.inp & sys.out files

Open the directory:

```
cd Scratch/cp2k/mapi/nvt-200K
```

Backup files:

```
cp sys.inp sys_142000_3005.inp && cp sys.out sys_142000_3005.out
```

<img src="../nvt-200K/80ps_backup.png" alt="70ps_backup.png" style="width:60%;" />

<img src="../nvt-200K/80ps_backup_check.png" alt="80ps_backup_check" style="width:60%;" />

2. Make new sys.inp file from sys-1.restart file

```
cp sys-1.restart sys.inp
```

<img src="../nvt-200K/80ps_make_inp.png" alt="80ps_make_inp" style="width:60%;" />

3. Check the new sys.inp file, the STEP_START_VAL should be 142000:

```
nano sys.inp
```

<img src="../nvt-200K/80ps_check_inp.png" alt="80ps_check_inp" style="width:60%;" />

4. Submit the job again by the command:

```
qsub job.pbs
```

<img src="../nvt-200K/80ps_qsub.png" alt="80ps_qsub" style="width:60%;" />

Check job status:

```
qstat
```

<img src="../nvt-200K/80ps_qw.png" alt="80ps_qw" style="width:60%;" />

### 90 ps

The AIMD simulation record of 90 ps at 200 K.

1. Backup sys.inp & sys.out files

Open the directory:

```
cd Scratch/cp2k/mapi/nvt-200K
```

Backup files:

```
cp sys.inp sys_162000_0206.inp && cp sys.out sys_162000_0206.out
```

<img src="../nvt-200K/90ps_backup.png" alt="90ps_backup.png" style="width:60%;" />

<img src="../nvt-200K/90ps_backup_check.png" alt="90ps_backup_check" style="width:60%;" />

2. Make new sys.inp file from sys-1.restart file

```
cp sys-1.restart sys.inp
```

<img src="../nvt-200K/90ps_make_inp.png" alt="90ps_make_inp" style="width:60%;" />

3. Check the new sys.inp file, the STEP_START_VAL should be 162000:

```
nano sys.inp
```

<img src="../nvt-200K/90ps_check_inp.png" alt="90ps_check_inp" style="width:60%;" />

4. Submit the job again by the command:

```
qsub job.pbs
```

<img src="../nvt-200K/90ps_qsub.png" alt="90ps_qsub" style="width:60%;" />

Check job status:

```
qstat
```

<img src="../nvt-200K/90ps_qw.png" alt="90ps_qw" style="width:60%;" />

### 100 ps

The AIMD simulation record of 100 ps at 200 K.

1. Backup sys.inp & sys.out files

Open the directory:

```
cd Scratch/cp2k/mapi/nvt-200K
```

Backup files:

```
cp sys.inp sys_182000_0306.inp && cp sys.out sys_182000_0306.out
```

<img src="../nvt-200K/100ps_backup.png" alt="100ps_backup.png" style="width:60%;" />

<img src="../nvt-200K/100ps_backup_check.png" alt="100ps_backup_check" style="width:60%;" />

2. Make new sys.inp file from sys-1.restart file

```
cp sys-1.restart sys.inp
```

<img src="../nvt-200K/100ps_make_inp.png" alt="100ps_make_inp" style="width:60%;" />

3. Check the new sys.inp file, the STEP_START_VAL should be 182000:

```
nano sys.inp
```

<img src="../nvt-200K/100ps_check_inp.png" alt="100ps_check_inp" style="width:60%;" />

4. Submit the job again by the command:

```
qsub job.pbs
```

<img src="../nvt-200K/100ps_qsub.png" alt="100ps_qsub" style="width:60%;" />

Check job status:

```
qstat
```

<img src="../nvt-200K/100ps_qw.png" alt="100ps_qw" style="width:60%;" />

<img src="../nvt-200K/100ps_running.png" alt="100ps_running" style="width:60%;" />

### 110 ps

The AIMD simulation record of 110 ps at 200 K.

1. Backup sys.inp & sys.out files

Open the directory:

```
cd Scratch/cp2k/mapi/nvt-200K
```

Backup files:

```
cp sys.inp sys_202000_0406.inp && cp sys.out sys_202000_0406.out
```

<img src="../nvt-200K/110ps_backup.png" alt="110ps_backup.png" style="width:60%;" />

<img src="../nvt-200K/110ps_backup_check.png" alt="110ps_backup_check" style="width:60%;" />

2. Make new sys.inp file from sys-1.restart file

```
cp sys-1.restart sys.inp
```

<img src="../nvt-200K/110ps_make_inp.png" alt="110ps_make_inp" style="width:60%;" />

3. Check the new sys.inp file, the STEP_START_VAL should be 202000:

```
nano sys.inp
```

<img src="../nvt-200K/110ps_check_inp.png" alt="110ps_check_inp" style="width:60%;" />

4. Submit the job again by the command:

```
qsub job.pbs
```

<img src="../nvt-200K/110ps_qsub.png" alt="110ps_qsub" style="width:60%;" />

Check job status:

```
qstat
```

<img src="../nvt-200K/110ps_qw.png" alt="110ps_qw" style="width:60%;" />

### 120 ps

The AIMD simulation record of 120 ps at 200 K.

1. Backup sys.inp & sys.out files

Open the directory:

```
cd Scratch/cp2k/mapi/nvt-200K
```

Backup files:

```
cp sys.inp sys_222000_0406.inp && cp sys.out sys_222000_0406.out
```

<img src="../nvt-200K/120ps_backup.png" alt="120ps_backup.png" style="width:60%;" />

<img src="../nvt-200K/120ps_backup_check.png" alt="120ps_backup_check" style="width:60%;" />

2. Make new sys.inp file from sys-1.restart file

```
cp sys-1.restart sys.inp
```

<img src="../nvt-200K/120ps_make_inp.png" alt="120ps_make_inp" style="width:60%;" />

3. Check the new sys.inp file, the STEP_START_VAL should be 222000:

```
nano sys.inp
```

<img src="../nvt-200K/120ps_check_inp.png" alt="120ps_check_inp" style="width:60%;" />

4. Submit the job again by the command:

```
qsub job.pbs
```

<img src="../nvt-200K/120ps_qsub.png" alt="120ps_qsub" style="width:60%;" />

Check job status:

```
qstat
```

<img src="../nvt-200K/120ps_qw.png" alt="100ps_qw" style="width:60%;" />

<img src="../nvt-200K/120ps_running.png" alt="120ps_running" style="width:60%;" />

### 130 ps

The AIMD simulation record of 130 ps at 200 K.

1. Backup sys.inp & sys.out files

Open the directory:

```
cd Scratch/cp2k/mapi/nvt-200K
```

Backup files:

```
cp sys.inp sys_242000_1706.inp && cp sys.out sys_242000_1706.out
```

<img src="../nvt-200K/130ps_backup.png" alt="130ps_backup.png" style="width:60%;" />

<img src="../nvt-200K/130ps_backup_check.png" alt="130ps_backup_check" style="width:60%;" />

2. Make new sys.inp file from sys-1.restart file

```
cp sys-1.restart sys.inp
```

<img src="../nvt-200K/130ps_make_inp.png" alt="130ps_make_inp" style="width:60%;" />

3. Check the new sys.inp file, the STEP_START_VAL should be 242000:

```
nano sys.inp
```

<img src="../nvt-200K/130ps_check_inp.png" alt="130ps_check_inp" style="width:60%;" />

4. Submit the job again by the command:

```
qsub job.pbs
```

<img src="../nvt-200K/130ps_qsub.png" alt="130ps_qsub" style="width:60%;" />

Check job status:

```
qstat
```

<img src="../nvt-200K/130ps_qw.png" alt="130ps_qw" style="width:60%;" />

<img src="../nvt-200K/130ps_running.png" alt="130ps_running" style="width:60%;" />

### 140 ps

The AIMD simulation record of 140 ps at 200 K.

1. Backup sys.inp & sys.out files

Open the directory:

```
cd Scratch/cp2k/mapi/nvt-200K
```

Backup files:

```
cp sys.inp sys_262000_1706.inp && cp sys.out sys_262000_1706.out
```

<img src="../nvt-200K/140ps_backup.png" alt="140ps_backup.png" style="width:60%;" />

<img src="../nvt-200K/140ps_backup_check.png" alt="140ps_backup_check" style="width:60%;" />

2. Make new sys.inp file from sys-1.restart file

```
cp sys-1.restart sys.inp
```

<img src="../nvt-200K/140ps_make_inp.png" alt="140ps_make_inp" style="width:60%;" />

3. Check the new sys.inp file, the STEP_START_VAL should be 262000:

```
nano sys.inp
```

<img src="../nvt-200K/140ps_check_inp.png" alt="140ps_check_inp" style="width:60%;" />

4. Submit the job again by the command:

```
qsub job.pbs
```

<img src="../nvt-200K/140ps_qsub.png" alt="140ps_qsub" style="width:60%;" />

Check job status:

```
qstat
```

<img src="../nvt-200K/140ps_running.png" alt="140ps_running" style="width:60%;" />

### 150 ps

The AIMD simulation record of 150 ps at 200 K.

1. Backup sys.inp & sys.out files

Open the directory:

```
cd Scratch/cp2k/mapi/nvt-200K
```

Backup files:

```
cp sys.inp sys_282000_1906.inp && cp sys.out sys_282000_1906.out
```

<img src="../nvt-200K/150ps_backup.png" alt="150ps_backup.png" style="width:60%;" />

<img src="../nvt-200K/150ps_backup_check.png" alt="150ps_backup_check" style="width:60%;" />

2. Make new sys.inp file from sys-1.restart file

```
cp sys-1.restart sys.inp
```

<img src="../nvt-200K/150ps_make_inp.png" alt="150ps_make_inp" style="width:60%;" />

3. Check the new sys.inp file, the STEP_START_VAL should be 282000:

```
nano sys.inp
```

<img src="../nvt-200K/150ps_check_inp.png" alt="150ps_check_inp" style="width:60%;" />

4. Submit the job again by the command:

```
qsub job.pbs
```

<img src="../nvt-200K/150ps_qsub.png" alt="150ps_qsub" style="width:60%;" />

Check job status:

```
qstat
```

<img src="../nvt-200K/150ps_qw.png" alt="150ps_qw" style="width:60%;" />

<img src="../nvt-200K/150ps_running.png" alt="150ps_running" style="width:60%;" />

### 160 ps

The AIMD simulation record of 160 ps at 200 K.

1. Backup sys.inp & sys.out files

Open the directory:

```
cd Scratch/cp2k/mapi/nvt-200K
```

Backup files:

```
cp sys.inp sys_302000_2106.inp && cp sys.out sys_302000_2106.out && ls
```

<img src="../nvt-200K/160ps_backup.png" alt="160ps_backup.png" style="width:60%;" />

<img src="../nvt-200K/160ps_backup_check.png" alt="160ps_backup_check" style="width:60%;" />

2. Make new sys.inp file from sys-1.restart file

```
cp sys-1.restart sys.inp
```

<img src="../nvt-200K/160ps_make_inp.png" alt="160ps_make_inp" style="width:60%;" />

3. Check the new sys.inp file, the STEP_START_VAL should be 302000:

```
nano sys.inp
```

<img src="../nvt-200K/160ps_check_inp.png" alt="160ps_check_inp" style="width:60%;" />

4. Submit the job again by the command:

```
qsub job.pbs
```

<img src="../nvt-200K/160ps_qsub.png" alt="160ps_qsub" style="width:60%;" />

Check job status:

```
qstat
```

<img src="../nvt-200K/160ps_qw.png" alt="160ps_qw" style="width:60%;" />

<img src="../nvt-200K/160ps_running.png" alt="160ps_running" style="width:60%;" />

### 170 ps

The AIMD simulation record of 170 ps at 200 K.

1. Backup sys.inp & sys.out files

Open the directory:

```
cd Scratch/cp2k/mapi/nvt-200K
```

Backup files:

```
cp sys.inp sys_322000_2206.inp && cp sys.out sys_322000_2206.out && ls
```

<img src="../nvt-200K/170ps_backup.png" alt="170ps_backup.png" style="width:60%;" />

<img src="../nvt-200K/170ps_backup_check.png" alt="170ps_backup_check" style="width:60%;" />

2. Make new sys.inp file from sys-1.restart file

```
cp sys-1.restart sys.inp
```

<img src="../nvt-200K/170ps_make_inp.png" alt="170ps_make_inp" style="width:60%;" />

3. Check the new sys.inp file, the STEP_START_VAL should be 322000:

```
nano sys.inp
```

<img src="../nvt-200K/170ps_check_inp.png" alt="170ps_check_inp" style="width:60%;" />

4. Submit the job again by the command:

```
qsub job.pbs
```

<img src="../nvt-200K/170ps_qsub.png" alt="170ps_qsub" style="width:60%;" />

Check job status:

```
qstat
```

<img src="../nvt-200K/170ps_qw.png" alt="170ps_qw" style="width:60%;" />

<img src="../nvt-200K/170ps_running.png" alt="170ps_running" style="width:60%;" />

### 180 ps

The AIMD simulation record of 180 ps at 200 K.

1. Backup sys.inp & sys.out files

Open the directory:

```
cd Scratch/cp2k/mapi/nvt-200K
```

Backup files:

```
cp sys.inp sys_342000_2206.inp && cp sys.out sys_342000_2206.out && ls
```

<img src="../nvt-200K/180ps_backup.png" alt="180ps_backup.png" style="width:60%;" />

<img src="../nvt-200K/180ps_backup_check.png" alt="180ps_backup_check" style="width:60%;" />

2. Make new sys.inp file from sys-1.restart file

```
cp sys-1.restart sys.inp
```

<img src="../nvt-200K/180ps_make_inp.png" alt="180ps_make_inp" style="width:60%;" />

3. Check the new sys.inp file, the STEP_START_VAL should be 342000:

```
nano sys.inp
```

<img src="../nvt-200K/180ps_check_inp.png" alt="180ps_check_inp" style="width:60%;" />

4. Submit the job again by the command:

```
qsub job.pbs
```

<img src="../nvt-200K/180ps_qsub.png" alt="180ps_qsub" style="width:60%;" />

Check job status:

```
qstat
```

<img src="../nvt-200K/180ps_qw.png" alt="180ps_qw" style="width:60%;" />

<img src="../nvt-200K/180ps_running.png" alt="180ps_running" style="width:60%;" />

### 190 ps

The AIMD simulation record of 190 ps at 200 K. This job was run on Cirrus. The script used to submit the job was:

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

# Load cp2k
module load cp2k

# Change to the submission directory
cd $SLURM_SUBMIT_DIR

#Ensure that no libraries are inadvertently using threading
export OMP_NUM_THREADS=1

# Launch the parallel job
srun cp2k.popt sys.inp > sys.out
```



1. Backup sys.inp & sys.out files

Open the directory:

```
cd mapi/nvt-200K
```

Backup files:

```
cp sys.inp sys_362000_1308.inp && cp sys.out sys_362000_1308.out && ls
```

<img src="../nvt-200K/190ps_backup.png" alt="190ps_backup.png" style="width:60%;" />

<img src="../nvt-200K/190ps_backup_check.png" alt="190ps_backup_check" style="width:60%;" />

2. Make new sys.inp file from `sys-1.restart` file. Check the new `sys.inp` file, the STEP_START_VAL should be 382000:

```
cp sys-1.restart sys.inp && nano sys.inp
```

<img src="../nvt-200K/200ps_make_inp.png" alt="200ps_make_inp" style="width:60%;" />

<img src="../nvt-200K/200ps_check_inp.png" alt="200ps_check_inp" style="width:60%;" />

3. Submit the job and check the job status by the command:

```
sbatch job.slurm && squeue -u uonlei
```

<img src="../nvt-200K/200ps_sbatch.png" alt="200ps_sbatch" style="width:60%;" />

```
squeue -u uonlei
```

<img src="../nvt-200K/200ps_running.png" alt="200ps_running" style="width:60%;" />

### 200 ps

1. Backup sys.inp & sys.out files

Open the directory:

```
cd mapi/nvt-200K
```

Backup files:

```
cp sys.inp sys_382000_1508.inp && cp sys.out sys_382000_1508.out && ls
```

<img src="../nvt-200K/200ps_backup.png" alt="200ps_backup.png" style="width:60%;" />

<img src="../nvt-200K/200ps_backup_check.png" alt="200ps_backup_check" style="width:60%;" />

2. Make new sys.inp file from `sys-1.restart` file. Check the new `sys.inp` file, the STEP_START_VAL should be 382000:

```
cp sys-1.restart sys.inp && nano sys.inp
```

<img src="../nvt-200K/200ps_make_inp.png" alt="190ps_make_inp" style="width:60%;" />

<img src="../nvt-200K/200ps_check_inp.png" alt="200ps_check_inp" style="width:60%;" />

3. Submit the job and check the job status by the command:

```
sbatch job.slurm && squeue -u uonlei
```

<img src="../nvt-200K/200ps_sbatch.png" alt="200ps_sbatch" style="width:60%;" />

```
squeue -u uonlei
```

<img src="../nvt-200K/190ps_running.png" alt="190ps_running" style="width:60%;" />

### 210 ps

1. Backup sys.inp & sys.out files

Open the directory:

```
cd mapi/nvt-200K
```

Backup files:

```
cp sys.inp sys_402000_1608.inp && cp sys.out sys_402000_1608.out && ls
```

<img src="../nvt-200K/210ps_backup.png" alt="210ps_backup.png" style="width:60%;" />

<img src="../nvt-200K/210ps_backup_check.png" alt="210ps_backup_check" style="width:60%;" />

2. Make new sys.inp file from `sys-1.restart` file. Check the new `sys.inp` file, the STEP_START_VAL should be 402000:

```
cp sys-1.restart sys.inp && nano sys.inp
```

<img src="../nvt-200K/210ps_make_inp.png" alt="190ps_make_inp" style="width:60%;" />

<img src="../nvt-200K/210ps_check_inp.png" alt="190ps_check_inp" style="width:60%;" />

3. Submit the job and check the job status by the command:

```
sbatch job.slurm && squeue -u uonlei
```

<img src="../nvt-200K/210ps_sbatch.png" alt="190ps_sbatch" style="width:60%;" />

```
squeue -u uonlei
```

<img src="../nvt-200K/210ps_running.png" alt="190ps_running" style="width:60%;" />

### 220 ps

1. Backup sys.inp & sys.out files

Open the directory:

```
cd mapi/nvt-200K
```

Backup files:

```
cp sys.inp sys_422000_1708.inp && cp sys.out sys_422000_1708.out && ls
```

<img src="../nvt-200K/220ps_backup.png" alt="220ps_backup.png" style="width:60%;" />

<img src="../nvt-200K/220ps_backup_check.png" alt="220ps_backup_check" style="width:60%;" />

2. Make new sys.inp file from `sys-1.restart` file. Check the new `sys.inp` file, the `STEP_START_VAL` should be ==422000==:

```
cp sys-1.restart sys.inp && nano sys.inp
```

<img src="../nvt-200K/220ps_make_inp.png" alt="220ps_make_inp" style="width:60%;" />

<img src="../nvt-200K/220ps_check_inp.png" alt="220ps_check_inp" style="width:60%;" />

3. Submit the job and check the job status by the command:

```
sbatch job.slurm && squeue -u uonlei
```

<img src="../nvt-200K/220ps_sbatch.png" alt="220ps_sbatch" style="width:60%;" />

```
squeue -u uonlei
```

<img src="../nvt-200K/220ps_running.png" alt="220ps_running" style="width:60%;" />

### 230 ps

1. Backup sys.inp & sys.out files

Open the directory:

```
cd mapi/nvt-200K
```

Backup files:

```
cp sys.inp sys_442000_1808.inp && cp sys.out sys_442000_1808.out && ls
```

<img src="../nvt-200K/230ps_backup.png" alt="230ps_backup.png" style="width:60%;" />

<img src="../nvt-200K/230ps_backup_check.png" alt="230ps_backup_check" style="width:60%;" />

2. Make new sys.inp file from `sys-1.restart` file. Check the new `sys.inp` file, the `STEP_START_VAL` should be ==442000==:

```
cp sys-1.restart sys.inp && nano sys.inp
```

<img src="../nvt-200K/230ps_make_inp.png" alt="230ps_make_inp" style="width:60%;" />

<img src="../nvt-200K/230ps_check_inp.png" alt="230ps_check_inp" style="width:60%;" />

3. Submit the job and check the job status by the command:

```
sbatch job.slurm && squeue -u uonlei
```

<img src="../nvt-200K/230ps_sbatch.png" alt="230ps_sbatch" style="width:60%;" />

```
squeue -u uonlei
```

<img src="../nvt-200K/230ps_running.png" alt="230ps_running" style="width:60%;" />

### 240 ps

1. Backup sys.inp & sys.out files

Open the directory:

```
cd mapi/nvt-200K
```

Backup files:

```
cp sys.inp sys_462000_2008.inp && cp sys.out sys_462000_2008.out && ls
```

<img src="../nvt-200K/240ps_backup.png" alt="240ps_backup.png" style="width:60%;" />

<img src="../nvt-200K/240ps_backup_check.png" alt="240ps_backup_check" style="width:60%;" />

2. Make new sys.inp file from `sys-1.restart` file. Check the new `sys.inp` file, the `STEP_START_VAL` should be ==462000==:

```
cp sys-1.restart sys.inp && nano sys.inp
```

<img src="../nvt-200K/240ps_make_inp.png" alt="240ps_make_inp" style="width:60%;" />

<img src="../nvt-200K/240ps_check_inp.png" alt="240ps_check_inp" style="width:60%;" />

3. Submit the job and check the job status by the command:

```
sbatch job.slurm && squeue -u uonlei
```

<img src="../nvt-200K/240ps_sbatch.png" alt="240ps_sbatch" style="width:60%;" />

### 250 ps

Change to the working directory.

```shell
cd mapi/nvt-200K
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

Submit the job by the `resub.slurm` script:

```shel
sbatch resub.slurm
```

<img src="../nvt-200K/250ps_sbatch.png" alt="250ps_sbatch" style="width:60%;" />



Check the new `sys.inp` file, the `STEP_START_VAL` should be ==482000==:

```
nano sys.inp
```

<img src="../nvt-200K/250ps_check_inp.png" alt="250ps_check_inp" style="width:60%;" />

Check the job status:

```shell
squeue -u uonlei
```

<img src="../nvt-200K/250ps_running.png" alt="250ps_running" style="width:60%;" />

### 260 ps

Change to the working directory.

```shell
cd mapi/nvt-200K
```



Submit the job by the `resub.slurm` script:

```shel
sbatch resub.slurm
```

<img src="../nvt-200K/260ps_sbatch.png" alt="260ps_sbatch" style="width:60%;" />

Check the job status:

```shell
squeue -u uonlei
```

<img src="../nvt-200K/260ps_running.png" alt="260ps_running" style="width:60%;" />

### 270 ps

Change to the working directory.

```shell
cd mapi/nvt-200K && sbatch resub.slurm
```

Check the job status:

```shell
squeue -u uonlei
```

<img src="../nvt-200K/270ps_sbatch.png" alt="270ps_sbatch" style="width:60%;" />

### 280 ps

Change to the working directory.

```shell
cd mapi/nvt-200K && sbatch resub.slurm
```

<img src="../nvt-200K/280ps_sbatch.png" alt="280ps_sbatch" style="width:60%;" />

Check the job status:

```shell
squeue -u uonlei
```

<img src="../nvt-200K/280ps_running.png" alt="280ps_running" style="width:60%;" />

The job did not finish within the time limit and stopped. The job did not run correctly when submitted again due to the wavefunction file   from last unfinished job is not complete. 

Firstly, identify the problem. The situation can happen due to the longer time taken by the job (the SCF take longer to converge due to the setting) or due to a problem of HPC (e.g. the hard disk problem).

Firstly, check the output file:

```
less sys-1.ener
```

Go to the end of the file by `shift+g`. The energy of the system should be stable if the simulation went well.

Check the output file to see how many steps did the job taken to converge:

```
grep "outer SCF l" sys.out
```

At last check the movie to see if all the steps are normal or not.

If the problem is confirmed to be the HPC. Use the backup wavefunction `sys-RESTART.wfn.bak-1` to continue the calculation:

```
cp sys-RESTART.wfn.bak-1 sys-RESTART.wfn
```

 Run a short MD job with 440 steps to finish the 280 ps job.

<img src="../nvt-200K/280ps_resub_check_inp.png" alt="280ps_resub_check_inp" style="width:60%;" />

The jobs print position information every 10 steps, the job stopped at step 561560 according to the `sys.inp` file made from `sys-1.restart`. Therefore, the 56157 steps’ information (including the first equilibrium position) at the top should be kept only. Each step has 195 lines, use `head` to keep the first 56157 &times; 195 = 10950615 lines in the position, force and velocity trajectory files. 

```
head -n 10950615 sys-pos-1.bak.xyz > sys-pos-1.xyz && head -n 10950615 sys-frc-1.bak.xyz > sys-frc-1.xyz && head -n 10950615 sys-vel-1.bak.xyz > sys-vel-1.xyz
```

<img src="../nvt-200K/280ps_resub_clean_xyz.png" alt="280ps_resub__clean_xyz" style="width:60%;" />

<img src="../nvt-200K/280ps_resub_sbatch.png" alt="280ps_resub_sbatch" style="width:60%;" />

Check the results of the short run, confirm the job continued as expected.

The trajectory breaks at the steps 56157 - 56158 which is due to only the first 56157 steps were kept. The 440 steps’ data were added to the backup trajectory files by the following steps.

Save the last 440 steps data into new files:

```shell
tail -n +10950616 sys-pos-1.xyz > sys-pos-1a.xyz && tail -n +10950616 sys-frc-1.xyz > sys-frc-1a.xyz && tail -n +10950616 sys-vel-1.xyz > sys-vel-1a.xyz
```

Add the new data to the backup files:

```shell
cat sys-pos-1a.xyz >> sys-pos-1.bak.xyz && cat sys-frc-1a.xyz >> sys-frc-1.bak.xyz && cat sys-vel-1a.xyz >> sys-vel-1.bak.xyz
```

Replace the trajectories so that the future jobs write the trajectory correctly.

```shell
cp sys-pos-1.bak.xyz sys-pos-1.xyz && cp sys-frc-1.bak.xyz sys-frc-1.xyz && cp sys-vel-1.bak.xyz sys-vel-1.xyz
```

The current steps is 562270.

### 290 ps

Change to the working directory.

```shell
cd ~/mapi/nvt-200K 
```

1. Backup files:

```
cp sys.inp sys_562000_0909.inp && cp sys.out sys_562000_0909.out && ls
```

<img src="../nvt-200K/290ps_backup.png" alt="290ps_backup.png" style="width:60%;" />

<img src="../nvt-200K/290ps_backup_check.png" alt="290ps_backup_check" style="width:60%;" />

2. Make new sys.inp file from `sys-1.restart` file. Check the new `sys.inp` file, the STEP_START_VAL should be 382000:

```
cp sys-1.restart sys.inp && nano sys.inp
```

<img src="../nvt-200K/290ps_make_inp.png" alt="290ps_make_inp" style="width:60%;" />

<img src="../nvt-200K/290ps_check_inp.png" alt="290ps_check_inp" style="width:60%;" />

3. Submit the job and check the job status by the command:

```
sbatch job.slurm && squeue -u uonlei
```

<img src="../nvt-200K/290ps_sbatch.png" alt="290ps_sbatch" style="width:60%;" />

### 300 ps

Change to the working directory.

```shell
cd ~/mapi/nvt-200K 
```

1. Backup files:

```
cp sys.inp sys_582000_1109.inp && cp sys.out sys_582000_1109.out && ls
```

<img src="../nvt-200K/300ps_backup.png" alt="300ps_backup.png" style="width:60%;" />

<img src="../nvt-200K/300ps_backup_check.png" alt="300ps_backup_check" style="width:60%;" />

2. Make new sys.inp file from `sys-1.restart` file. Check the new `sys.inp` file, the STEP_START_VAL should be 382000:

```
cp sys-1.restart sys.inp && nano sys.inp
```

<img src="../nvt-200K/300ps_make_inp.png" alt="300ps_make_inp" style="width:60%;" />

<img src="../nvt-200K/300ps_check_inp.png" alt="300ps_check_inp" style="width:60%;" />

3. Submit the job and check the job status by the command:

```
sbatch job.slurm && squeue -u uonlei
```

<img src="../nvt-200K/300ps_sbatch.png" alt="300ps_sbatch" style="width:60%;" />

### 310 ps

Change to the working directory and submit the job by the `resub.slurm` script.

```shell
cd ~/mapi/nvt-200K && sbatch resub.slurm
```

Check the job status:

```shell
squeue -u uonlei
```

<img src="../nvt-200K/310ps_sbatch.png" alt="310ps_sbatch" style="width:60%;" />

The steps after 61750 became very slow, the job was manually stopped. The steps not finished will be calculated in the 320 ps job. The trajectory file was checked, no abnormal steps were noted.

### 320 ps

Change to the working directory and submit the job by the `resub.slurm` script.

```shell
cd ~/mapi/nvt-200K
```

Make the `sys-RESTART.wfn` from `sys-RESTART.wfn.bak-1`:

```
cp sys-RESTART.wfn.bak-1 sys-RESTART.wfn
```

<img src="../nvt-200K/310ps_resub_reset_wfn.png" alt="310ps_resub_reset_wfn" style="width:60%;" />

Check the `sys.inp` file:

<img src="../nvt-200K/320ps_check_inp.png" alt="320ps_check_inp" style="width:60%;" />

Submit the job again:

```
sbatch job.slurm
```

<img src="../nvt-200K/320ps_sbatch.png" alt="320ps_sbatch" style="width:60%;" />

### 330 ps

Change to the working directory and submit the job by the `resub.slurm` script.

```shell
cd ~/mapi/nvt-200K
```

Backup files:

```shell
cp sys.inp sys_642000_1809.inp && cp sys.out sys_642000_1809.out && ls
```

<img src="../nvt-200K/330ps_backup.png" alt="330ps_backup" style="width:60%;" />

<img src="../nvt-200K/330ps_backup_check.png" alt="330ps_backup_check" style="width:60%;" />

Make `.inp` file from `sys-1.restart`:

```shell
cp sys-1.restart sys.inp
```

<img src="../nvt-200K/330ps_make_inp.png" alt="330ps_make_inp" style="width:60%;" />

Check the `sys.inp` file:

<img src="../nvt-200K/330ps_check_inp.png" alt="330ps_check_inp" style="width:60%;" />

Submit the job again:

```
sbatch job.slurm
```

<img src="../nvt-200K/330ps_sbatch.png" alt="330ps_sbatch" style="width:60%;" />

### 340 ps

Change to the working directory.

```shell
cd ~/mapi/nvt-200K && sbatch resub.slurm
```

Check the job status:

```shell
squeue -u uonlei
```

<img src="../nvt-200K/340ps_sbatch.png" alt="340ps_sbatch" style="width:60%;" />

<img src="../nvt-200K/340ps_running.png" alt="340ps_running" style="width:60%;" />

### 350 ps

Change to the working directory.

```shell
cd ~/mapi/nvt-200K && sbatch resub.slurm
```

Check the job status:

```shell
squeue -u uonlei
```

<img src="../nvt-200K/350ps_sbatch.png" alt="350ps_sbatch" style="width:60%;" />

### 360 ps

Change to the working directory.

```shell
cd ~/mapi/nvt-200K && sbatch resub.slurm
```

Check the job status:

```shell
squeue -u uonlei
```

<img src="../nvt-200K/360ps_sbatch.png" alt="360ps_sbatch" style="width:60%;" />

### 370 ps

Change to the working directory.

```shell
cd ~/mapi/nvt-200K && sbatch resub.slurm
```

Check the job status:

```shell
squeue -u uonlei
```

<img src="../nvt-200K/370ps_sbatch.png" alt="370ps_sbatch" style="width:60%;" />

### 380 ps

Change to the working directory.

```shell
cd ~/mapi/nvt-200K && sbatch resub.slurm
```

Check the job status:

```shell
squeue -u uonlei
```

<img src="../nvt-200K/380ps_sbatch.png" alt="380ps_sbatch" style="width:60%;" />

### 390 ps

Change to the working directory.

```shell
cd ~/mapi/nvt-200K && sbatch resub.slurm
```

Check the job status:

```shell
squeue -u uonlei
```

<img src="../nvt-200K/390ps_sbatch.png" alt="390ps_sbatch" style="width:60%;" />

### 400 ps

Change to the working directory.

```shell
cd ~/mapi/nvt-200K && sbatch resub.slurm
```

Check the job status:

```shell
squeue -u uonlei
```

<img src="../nvt-200K/400ps_sbatch.png" alt="400ps_sbatch" style="width:60%;" />

### 410 ps

Change to the working directory.

```shell
cd ~/mapi/nvt-200K && sbatch resub.slurm
```

Check the job status:

```shell
squeue -u uonlei
```

<img src="../nvt-200K/410ps_sbatch.png" alt="410ps_sbatch" style="width:60%;" />

<img src="../nvt-200K/410ps_squeue.png" alt="410ps_squeue" style="width:60%;" />

### 420 ps

Change to the working directory.

```shell
cd ~/mapi/nvt-200K && sbatch resub.slurm
```

Check the job status:

```shell
squeue -u uonlei
```

<img src="../nvt-200K/420ps_sbatch.png" alt="420ps_sbatch" style="width:60%;" />

### 430 ps

Change to the working directory.

```shell
cd ~/mapi/nvt-200K && sbatch resub.slurm
```

Check the job status:

```shell
squeue -u uonlei
```

<img src="../nvt-200K/430ps_sbatch.png" alt="430ps_sbatch" style="width:60%;" />

### 440 ps

Change to the working directory.

```shell
cd ~/mapi/nvt-200K && sbatch resub.slurm
```

Check the job status:

```shell
squeue -u uonlei
```

<img src="../nvt-200K/440ps_sbatch.png" alt="440ps_sbatch" style="width:60%;" />

### 450 ps

Change to the working directory.

```shell
cd ~/mapi/nvt-200K && sbatch resub.slurm
```

Check the job status:

```shell
squeue -u uonlei
```

<img src="../nvt-200K/450ps_sbatch.png" alt="450ps_sbatch" style="width:60%;" />

### 460 ps

Change to the working directory.

```shell
cd ~/mapi/nvt-200K && sbatch resub.slurm
```

Check the job status:

```shell
squeue -u uonlei
```

<img src="../nvt-200K/460ps_sbatch.png" alt="460ps_sbatch" style="width:60%;" />

### 470 ps

Change to the working directory.

```shell
cd ~/mapi/nvt-200K && sbatch resub.slurm
```

Check the job status:

```shell
squeue -u uonlei
```

<img src="../nvt-200K/470ps_sbatch.png" alt="470ps_sbatch" style="width:60%;" />

### 480 ps

Change to the working directory.

```shell
cd ~/mapi/nvt-200K && sbatch resub.slurm
```

Check the job status:

```shell
squeue -u uonlei
```

<img src="../nvt-200K/480ps_sbatch.png" alt="480ps_sbatch" style="width:60%;" />

### 490 ps

Change to the working directory.

```shell
cd ~/mapi/nvt-200K && sbatch resub.slurm
```

Check the job status:

```shell
squeue -u uonlei
```

<img src="../nvt-200K/490ps_sbatch.png" alt="490ps_sbatch" style="width:60%;" />

### 500 ps

Change to the working directory.

```shell
cd ~/mapi/nvt-200K && sbatch resub.slurm
```

Check the job status:

```shell
squeue -u uonlei
```

<img src="../nvt-200K/500ps_sbatch.png" alt="500ps_sbatch" style="width:60%;" />

### 510 ps

Change to the working directory.

```shell
cd ~/mapi/nvt-200K && sbatch resub.slurm
```

Check the job status:

```shell
squeue -u uonlei
```

<img src="../nvt-200K/510ps_sbatch.png" alt="510ps_sbatch" style="width:60%;" />

### 520 ps

Change to the working directory.

```shell
cd ~/mapi/nvt-200K
```

1. Backup files:

```
cp sys.inp sys_1018000_2010.inp && cp sys.out sys_1018000_2010.out && ls
```

<img src="../nvt-200K/520ps_backup.png" alt="520ps_backup.png" style="width:60%;" />

<img src="../nvt-200K/520ps_backup_check.png" alt="520ps_backup_check" style="width:60%;" />

2. Make new sys.inp file from `sys-1.restart` file. Check the new `sys.inp` file, the STEP_START_VAL should be 382000:

```
cp sys-1.restart sys.inp && nano sys.inp
```

<img src="../nvt-200K/520ps_make_inp.png" alt="520ps_make_inp" style="width:60%;" />

<img src="../nvt-200K/520ps_check_inp.png" alt="520ps_check_inp" style="width:60%;" />

3. Submit the job and check the job status by the command:

```
sbatch job.slurm && squeue -u uonlei
```

<img src="../nvt-200K/520ps_sbatch.png" alt="520ps_sbatch" style="width:60%;" />



The job stopped at 1033000 due to the disk problem. 

### 530 ps

Continue the unexpectedly stopped job:

```shell
cp sys-RESTART.wfn.bak-1 sys-RESTART.wfn
```

1. Backup files:

```
cp sys.inp sys_1033000_2510.inp && cp sys.out sys_1033000_2510.out && ls
```

<img src="../nvt-200K/520ps_backup-1.png" alt="520ps_backup-1.png" style="width:60%;" />

<img src="../nvt-200K/520ps_backup_check-1.png" alt="520ps_backup_check-1" style="width:60%;" />

2. Make new sys.inp file from `sys-1.restart` file. Check the new `sys.inp` file, the STEP_START_VAL should be 382000:

```
cp sys-1.restart sys.inp && nano sys.inp
```

<img src="../nvt-200K/530ps_make_inp.png" alt="530ps_make_inp" style="width:60%;" />

<img src="../nvt-200K/530ps_check_inp.png" alt="530ps_check_inp" style="width:60%;" />

3. Submit the job and check the job status by the command:

```
sbatch job.slurm && squeue -u uonlei
```

<img src="../nvt-200K/530ps_sbatch.png" alt="530ps_sbatch" style="width:60%;" />

### 540 ps

Continue the unexpectedly stopped job:

```shell
cp sys-RESTART.wfn.bak-1 sys-RESTART.wfn
```

1. Backup files:

```
cp sys.inp sys_1060000_2710.inp && cp sys.out sys_1060000_2710.out && ls
```

<img src="../nvt-200K/540ps_backup.png" alt="540ps_backup.png" style="width:60%;" />

<img src="../nvt-200K/540ps_backup-check.png" alt="540ps_backup-check" style="width:60%;" />

2. Make new sys.inp file from `sys-1.restart` file. Check the new `sys.inp` file, the STEP_START_VAL should be 382000:

```
cp sys-1.restart sys.inp && nano sys.inp
```

<img src="../nvt-200K/540ps_make_inp.png" alt="540ps_make_inp" style="width:60%;" />

<img src="../nvt-200K/540ps_check_inp.png" alt="540ps_check_inp" style="width:60%;" />

3. Submit the job and check the job status by the command:

```
sbatch job.slurm && squeue -u uonlei
```

<img src="../nvt-200K/540ps_sbatch.png" alt="540ps_sbatch" style="width:60%;" />

### 550 ps

1. Backup files:

```
cp sys.inp sys_1082000_2710.inp && cp sys.out sys_1082000_2710.out && ls
```

<img src="../nvt-200K/550ps_backup.png" alt="550ps_backup.png" style="width:60%;" />

<img src="../nvt-200K/550ps_backup_check.png" alt="550ps_backup-check" style="width:60%;" />

2. Make new sys.inp file from `sys-1.restart` file. Check the new `sys.inp` file, the STEP_START_VAL should be 382000:

```
cp sys-1.restart sys.inp && nano sys.inp
```

<img src="../nvt-200K/550ps_make_inp.png" alt="550ps_make_inp" style="width:60%;" />

<img src="../nvt-200K/550ps_check_inp.png" alt="550ps_check_inp" style="width:60%;" />

3. Submit the job and check the job status by the command:

```
sbatch job.slurm && squeue -u uonlei
```

<img src="../nvt-200K/550ps_sbatch.png" alt="550ps_sbatch" style="width:60%;" />

### 560 ps

Change to the working directory.

```shell
cd ~/mapi/nvt-200K && sbatch resub.slurm
```

Check the job status:

```shell
squeue -u uonlei
```

<img src="../nvt-200K/560ps_sbatch.png" alt="560ps_sbatch" style="width:60%;" />

### 570 ps

Change to the working directory.

```shell
cd ~/mapi/nvt-200K && sbatch resub.slurm
```

Check the job status:

```shell
squeue -u uonlei
```

<img src="../nvt-200K/570ps_sbatch.png" alt="570ps_sbatch" style="width:60%;" />

### 580 ps

Change to the working directory.

```shell
cd ~/mapi/nvt-200K && sbatch resub.slurm
```

Check the job status:

```shell
squeue -u uonlei
```

<img src="../nvt-200K/580ps_sbatch.png" alt="580ps_sbatch" style="width:60%;" />

### 590 ps

Change to the working directory.

```shell
cd ~/mapi/nvt-200K && sbatch resub.slurm
```

Check the job status:

```shell
squeue -u uonlei
```

<img src="../nvt-200K/590ps_sbatch.png" alt="590ps_sbatch" style="width:60%;" />

### 600 ps

Change to the working directory.

```shell
cd ~/mapi/nvt-200K && sbatch resub.slurm
```

Check the job status:

```shell
squeue -u uonlei
```

<img src="../nvt-200K/600ps_sbatch.png" alt="600ps_sbatch" style="width:60%;" />
