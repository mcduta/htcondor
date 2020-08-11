# Docker containers for a HTCondor cluster on Kubernetes

The Docker images built from these files contain the HTCondor
distributed computing software (https://research.cs.wisc.edu/htcondor/manual/)
for High Throughput Computing.

## HTCondor configuration

The Docker images are designed to be used by Kubernetes to create and
control a compute cluster. The configuration of HTCondor is achieved
via Kubernetes configuration maps. There are 3 configurations to
fulfill a number of roles:

- the **Central Manager**, which runs the Master, Collector and
  Negotiator;

- the **Submission Service**, which runs the Master and the Scheduler
  Daemon (Schedd) and

- the **Job Executor**, which runs the Master and Started Daemon
  (Startd).

The normal user only interacts with the Submission Service. Jobs are
run by the Job Executors, which have resources of different
specifications (including GPUs).


## Docker images available

All Docker images contain the HTCondor software. The images fulfill a number of roles 

Starting with the Docker image **centos:7**, the following sequence of
Docker images are built (each one from the preceding):

- **htcondor**
- **htcondor-singularity**
- **htcondor-singularity-pam**
- **htcondor-singularity-pam-sshd**

The image **htcondor-singularity-cuda** is similar to
**htcondor-singularity** but built from
**nvidia/cuda:10.2-runtime-centos7** rather than **centos:7**,
therefore contains the Nvidia CUDA Toolkit.

## Docker images roles

| HTCondor   | Docker                    | Description        | Software    |
| Role       | Image                     |                    | Contained   |
|------------|---------------------------|--------------------|-------------|
| Central    | htcondor                  | Overall HTCondor   | HTCondor    |
| Manager    |                           | cluster manager    |             |
|            |                           |                    |             |
| Job        | htcondor-singularity      | CPU only           | HTCondor    |
| Execution  |                           | processing         | Singularity |
|            |                           |                    |             |
| Job        | htcondor-singularity-cuda | GPU                | HTCondor    |
| Execution  |                           | processing         | Singularity |
|            |                           |                    | CUDA        |
|            |                           |                    |             |
| Job        | htcondor-singularity-pam  | User service for   | HTCondor    |
| Submission | singularity-pam-sshd      | job submission and | Singularity |
|            |                           | monitoring         | PAM         |
|            |                           |                    | SSHD        |
