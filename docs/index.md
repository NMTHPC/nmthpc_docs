# New Mexico Tech High Performance Computing Documentation

Welcome to the New Mexico Tech High Performance Computing (NMTHPC) documentation. This guide provides comprehensive information about accessing, using, and optimizing your work on our HPC cluster.

## About NMTHPC

The High-Performance Computing system at New Mexico Tech – NMTHPC – was established through a competitive National Science Foundation Major Research Instrumentation (NSF-MRI) award (#2320162) awarded in 2023:   The award, MRI: Track 1 Acquisition of a High-Performance Computing System at New Mexico Tech, provides funding for both the HPC instrumentation framework and undergraduate participation in system administration. 

The driving centerpiece of NMTHPC will be the collaborative, transdisciplinary research that will result from having exponentially more resources available to our faculty and students. Over the course of the three year award, key research challenges the HPC system will be used to address include Seismogenic Processes and Hazards, Computational Biology and Bioinformatics, Computational Materials Science, and Multiphysics Groundwater Transport, Characterization, and Mapping. NMTHPC aims to continually expand the computational capabilities to adapt to the ever growing needs and challenges of our local, state and national stakeholders. Long-term, the NMTHPC team also aims to leverage the HPC system to develop curriculum and certificate programs in research computing and HPC system administration.

## System Overview

The HPC system funded by the MRI is designed to meet the wide range of research computing needs across campus. Key features of the AMD-based system include three types of compute nodes (standard, high memory, GPU), a parallel file system, and fast internal communication. 

(16) Standard Nodes (256 cores, 6 Gb RAM per core)
(3) High-Memory Nodes (256 cores, 9 Gb RAM per core)
(3) GPU Nodes (128 Cores, 6 Gb RAM per core, NVIDIA H100 GPUs w/ 16896 CUDA Cores)
(1) Login Node (128 cores, 6 Gb RAM per core)
(1) 373 Tb HDD Storage Server
(1) 500 Tb Parallel File System 


```{tip}
- New to HPC or NMTHPC? Start with our [Navigating the NMT HPC Documentation](./getting_started/navigating_docs.md) page
- Need additional help? Contact the support team. 
- Looking for specific software? Check our [Software Available on NMTHPC](./computing_environment/software.md) page
```

----

::::{dropdown} Click to show the full index for all documentation
:icon: list-unordered

```{toctree}
:maxdepth: 1
:caption: Getting Started

getting_started/navigating_docs
getting_started/accounts_login
getting_started/faq
getting_started/acknowledging_nmthpc
```

```{toctree}
:maxdepth: 1
:caption: Computing Environment

computing_environment/nodes_filesystems
computing_environment/software
computing_environment/data_transfer
computing_environment/monitoring_resources
computing_environment/partitions_qos
```

```{toctree}
:maxdepth: 1
:caption: Using NMTHPC

using_nmthpc/interactive_jobs
using_nmthpc/batch_jobs
using_nmthpc/job_arrays
using_nmthpc/gpu_jobs
```

```{toctree}
:maxdepth: 1
:caption: Software and Examples

software/software_overview
software/r
software/vasp
software/spack
```

```{toctree}
:maxdepth: 1
:caption: Programming tutorials

programming/coding-best-practices
programming/MPI-C
programming/MPI-Fortran
programming/OpenMP-C
programming/OpenMP-Fortran
programming/parallel-programming-fundamentals
programming/profiling-nvidia-gpu-performance
```

::::
