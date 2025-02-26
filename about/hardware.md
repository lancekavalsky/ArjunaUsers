---
layout: default
title: Cluster Architecture
parent: About Arjuna
nav_order: 1
---

# Cluster Architecture

## Head Node

When you ssh into Arjuna, you connect to the "Head Node", also known as c001. From this machine, you
can do the following

- Transfer files to/from Arjuna to a local machine (i.e. a compute node or another computer connected to the CMU network to which you have access)
- Download files from the internet
- Submit jobs to the cluster
- Monitor the status of existing jobs

**DO NOT** run compute jobs on the Head Node. Moreover, do not use the Head Node for anything other than submitting jobs to compute nodes. Unauthorized uses of the Head Node include, but are not limited to:

- Running Jupyter Notebooks to analyze data
- Webscraping
- Running Simulations

Authorized uses of the Head Node include, but are not limited to:

- Installing software for use on the compute nodes
- Moving data to and from Arjuna
- Submitting jobs

If the desired compute task is anything other than trivial operations required for job submission, which can only be run on the Head Node of Arjuna, it should be run on a worker node or elsewhere.

## Partitions

Partition can be specified by passing the `-p` or
`--partition` flag to `srun`, `sbatch` or `salloc`. By default jobs are submitted
to the *debug* partition.

Unless otherwise specified, jobs have a max runtime of 1 day and receive 1GB of
memory per requested CPU. See [Allocating Resources] for more information.

[Allocating Resources]: ../getting_started/slurm_intro.html#allocating-resources

| Partition | Count | Cores | Memory | Tmp Storage | Max Time |
|-----------|-------|-------|--------|-------------|----------|
| cpu       | 58    | 56    | 128 GB | 100 GB      | 7 days   |
| highmem   | 2     | 32    | 512 GB | 100 GB      | 7 days   |
| gpu       | 27    | 64    | 128 GB | 100 GB      | 14 days  |
| debug     | 2     | 64    | 128 GB | 100 GB      | 10 minutes |

For more information on the partitions and their default settings run
`scontrol show partitions` on Arjuna.

### GPU Nodes

Each GPU Nodes has 4 [K80 NVIDIA GPUs] available for usage. To request a gpu, use
the `--gres=gpu:N` flag, where `N` is the number of GPUs requested.

[K80 NVIDIA GPUs]: https://www.nvidia.com/en-gb/data-center/tesla-k80/

See [Generic Resource Scheduling](https://slurm.schedmd.com/gres.html) for more
information about requesting GPUs.

### Internet Access
Workers can not resolve [domain names](https://en.wikipedia.org/wiki/Domain_name), and as a result, most internet services may not function correctly. For example,

- Cloning a git repository from [github.com]()
- Downloading packages using `spack`, `pip`, `conda` or `Pkg.jl`
- Downloading files with `wget`, `rclone` or `curl` from the Internet

Users are encoraged to use the [Head Node](#head-node) for these tasks.

## Storage

Arjuna has 20 TB of [RAID 6] storage.

> Arjuna's RAID Storage is not meant for long-term storage of data. For long-term data storage, please use other resources such as CMU's unlimited google drive access.

[RAID 6]: https://en.wikipedia.org/wiki/Standard_RAID_levels#RAID_6

Users are allocated 350 GB of storage in their `/home` directory.

- If you exceed this, you have 7 days before further usage is blocked
- There is a hard cap at 500 GB

Once your grace period has elapsed (Or you exceed the hard limit), additional
writes will be blocked. *This may cause job failure*

To see your current disk usage: `quota -s`

### Removing Temporary Files

Temporary files generated by jobs can quickly consume storage space.
The following command will recursively delete all `*.gpw` files in your home directory (`~`).

```shell
find ~ -name `*.gpw` -delete
```

### Backing up Data

User data stored on Arjuna is **NOT** backed up by the Arjuna Admin Team.
Users are responsible for backing up their data. See [Data Backup] for additional
guidance.

[Data Backup]: ../getting_started/data_backup.html
