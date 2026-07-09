# Software available

Here we provide a summary of the software packages currently available on NMTHPC.



:::{admonition} Legend for software table
:class: tip

**(D)** Default module version **(G)** GPU-accelerated **(L)** License-restricted
:::

| Application           | Version(s)          | Description      |
| --------------------- | ------------------- | ---------------- |
| [Apptainer](https://apptainer.org) | 1.4.1 | Apptainer is a container platform for high-performance computing (HPC) that simplifies the creation and execution of containers, ensuring software components are encapsulated for portability and reproducibility. |
| [Aspect](https://aspect.geodynamics.org) | 3.0.0 (D) | Aspect is an extensible code written in C++ to support research in simulating convection in the solid Earth and elsewhere. It provides the geosciences with a well-documented and extensible code base for their research needs. |
| [BWA](https://bio-bwa.sourceforge.net) | 0.7.19 | BWA (Burrows-Wheeler Aligner) is a software package for mapping low-divergent DNA sequencing reads against large reference genomes. |
| [CMake](https://cmake.org) | 3.20.6 | CMake is a cross-platform build system generator that automates the configuration, generation, and building of software projects using compiler-independent configuration files. |
| [CP2K](https://www.cp2k.org) | 2024.3 (D) | CP2K is an open-source program for atomistic simulations of solid-state, liquid, molecular, periodic, material, crystal, and biological systems. It provides electronic structure calculations and molecular dynamics using methods such as density functional theory (DFT), Hartree–Fock, and classical force fields. |
| [Conda (via Miniforge3)](https://conda-forge.org/download/) | 24.3.0 (D) | Conda is provided through the Miniforge3 module for creating isolated software environments and managing packages. |
| [CPMD](https://github.com/CPMD-code) | 4.3 (D) | The CPMD code is a parallelized plane wave / pseudopotential implementation of Density Functional Theory, particularly designed for ab-initio molecular dynamics. | 
| [CUDA Toolkit](https://developer.nvidia.com/cuda-toolkit) (G) | 12.6 (via NVHPC 24.9) | The NVIDIA CUDA Toolkit includes GPU-accelerated libraries, debugging and optimization tools, a C/C++ compiler, and a runtime library to deploy your application. *Included in the `nvhpc/24.9` module. Future CUDA versions will only be available through the NVIDIA HPC module. |
| [D-Bus (dbus)](https://www.freedesktop.org/wiki/Software/dbus/) | 1.14.10 (D) | D-Bus is an inter-process communication (IPC) system that enables communication and coordination between software applications and system services on Linux. |
| [deal.II (dealii)](https://dealii.org) | 9.5.1, 9.6.2 (D) | deal.II is an open-source finite element library for developing computational simulations using adaptive finite elements, parallel computing, and modern numerical methods. |
| [FFTW](https://www.fftw.org) | 3.3.10 (D) | FFTW (Fastest Fourier Transform in the West) is an open-source library for computing discrete Fourier transforms (DFTs) in one or more dimensions. It provides highly optimized routines for scientific computing, signal processing, and high-performance applications. |
| [Funannotate](https://funannotate.readthedocs.io/en/latest/) | 1.8.17 | Funannotate is a genome annotation pipeline for predicting, annotating, and analyzing eukaryotic genomes using multiple integrated bioinformatics tools. |
| [gcc](https://gcc.gnu.org/) |  10.3.0, 13.3.0, 14.2.0 (D) |The GNU Compiler Collection includes front ends for C, C++, Objective-C, Fortran, Ada, Go, and D, as well as libraries for these languages (libstdc++,...). | 
| [Git](https://git-scm.com/downloads) | 2.47.3 (System-Installed) | Git is a distributed version control system that tracks changes in any set of computer files. |
| [Go](https://go.dev/doc/) | 1.22.8 | Go is an open-source programming language designed to help developers build efficient, reliable, and scalable software. It features fast compilation, built-in concurrency support, and a statically typed language that improves productivity while simplifying the development of modern applications. |
| [HDF5](https://www.hdfgroup.org/solutions/hdf5/) | 1.14.5 | HDF5 is a data model, library, and file format for storing and managing data. |
| [Intel oneAPI](https://www.intel.com/content/www/us/en/developer/tools/oneapi/oneapi-toolkit.html) | Compilers 2024.0.2, Classic Compilers 2021.10.0, MKL 2024.2.2, MPI 2021.14.0 | Intel oneAPI provides a collection of performance libraries, compilers, and development tools for building high-performance applications on CPUs and accelerators. |
| [Miniforge3](https://github.com/conda-forge/miniforge) (L) | 24.3.0 (D) | Miniforge3 is a minimal Python distribution that provides the Conda package and environment manager for creating isolated software environments and managing packages. |
| [NVIDIA HPC SDK (nvhpc)](https://developer.nvidia.com/hpc-sdk) (L) | 24.9 (D) | The NVIDIA HPC SDK is a comprehensive suite of compilers, libraries, and development tools for developing high-performance computing (HPC) applications on CPUs and NVIDIA GPUs. It includes NVIDIA C, C++, and Fortran compilers, CUDA, OpenACC and OpenMP support, optimized math libraries, and MPI for GPU-accelerated computing. | 
| [OpenBLAS](https://www.openblas.net/) | 0.3.25, 0.3.28 (D) | OpenBLAS is an optimized Basic Linear Algebra Subprograms (BLAS) library based on GotoBLAS 21.13 BSD version. | 
| [Open MPI](https://www.open-mpi.org/) | 4.0.7, 4.1.6, 5.0.5 (D) | The Open MPI Project is an open source Message Passing Interface implementation that is developed and maintained by a consortium of academic, research, and industry partners. |
| [ORCA](https://www.faccts.de/orca/) | 6.1.1 | ORCA is a powerful and versatile quantum chemistry software package, primarily developed by the group of Prof. Frank Neese. It is free for academic use, while commercial licenses are available through FACCTs. |
| [Python](https://www.python.org/) | 3.12.5 | Python is a high-level, general-purpose programming language. Its design philosophy emphasizes code readability with the use of significant indentation. |
| [QIIME 2](https://qiime2.org) | 2024.10 | QIIME 2 is an open-source microbiome bioinformatics platform for performing reproducible analysis of amplicon sequencing and other microbiome data. |
| [QuantumESPRESSO (quantum-espresso)](https://www.quantum-espresso.org) | 7.4 (D) | Quantum ESPRESSO is an integrated suite of open-source computer codes for electronic-structure calculations and materials modeling at the nanoscale. It is based on density-functional theory, plane waves, and pseudopotentials. |
| [R](https://www.r-project.org/) | 4.4.1 (D) | R is a programming language for statistical computing and graphics. |
| [ScaLAPACK (netlib-scalapack)](https://www.netlib.org/scalapack/) | 2.2.0 (D) | ScaLAPACK is a library of high-performance linear algebra routines for parallel distributed memory machines. ScaLAPACK solves dense and banded linear systems, least squares problems, eigenvalue problems, and singular value problems. |
| [Slurm](https://slurm.schedmd.com/overview.html) | 22.05.9, 23 (D) | Slurm (Simple Linux Utility for Resource Management) is an open-source workload manager and job scheduler used to allocate computing resources, schedule jobs, and manage batch and interactive workloads on HPC clusters. |
| [SRA Toolkit](https://github.com/ncbi/sra-tools) | 3.4.1 | SRA Toolkit is a collection of command-line tools for accessing, downloading, and processing sequencing data from the NCBI Sequence Read Archive (SRA). |
| [tcsh](https://www.tcsh.org) | 6.24.00 | tcsh is an enhanced version of the Berkeley C shell (csh) that provides a command-line interpreter with features such as command-line editing, programmable completion, history substitution, and scripting for Unix and Linux systems. |
| [tl-expected](https://vcpkg.io/en/package/tl-expected.html) | 1.1.0 (D) | tl-expected is a header-only C++ library that provides an implementation of std::expected, enabling expressive error handling and value-or-error semantics for C++ applications prior to C++23. |
| [Trilinos](https://trilinos.github.io) | 16.0.0 (D) | Trilinos is an advanced software framework designed to facilitate the development of high-performance scientific applications. Trilinos provides a comprehensive suite of libraries and tools that support a wide range of computational tasks, from linear algebra and optimization to differential equations and mesh generation. |
| [UCX](https://openucx.org) | 1.17.0, 1.18.0 (D) | UCX (Unified Communication X) is an open-source communication framework that provides high-performance, low-latency communication for HPC applications. It supports high-speed networks, GPUs, and shared-memory communication through a unified API. |
| [VASP](https://vasp.at/) (L) | 6.4.3 (D) | VASP (Vienna Ab initio Simulation Package) is a first-principles simulation package for performing electronic structure calculations and atomistic materials modeling using density functional theory (DFT), molecular dynamics, and related methods. |
| [WRF](https://github.com/wrf-model/WRF) | 4.6.1 | WRF (Weather Research and Forecasting Model) is a numerical weather prediction system designed for atmospheric research and operational forecasting. |
