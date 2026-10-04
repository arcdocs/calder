# Using the HPC

Aire and Calder are both University of Leeds HPC facilities, but they differ in hardware and software environment. Calder is the newer system, with more powerful CPU and GPU hardware (AMD EPYC 9555 processors and NVIDIA H200 GPUs, connected via NDR InfiniBand), while Aire remains the established platform (older AMD EPYC processors and NVIDIA L40S GPUs, connected via Omni-Path). For a full hardware comparison, see the [System Overview](../system/start.md).

Day-to-day usage is largely the same on both systems — both are managed by the Slurm job scheduler, and most commands and job scripts work the same way on each. Where there are differences that affect how you use the system (for example, GPU types, module systems, or storage paths), they are called out directly in the sections below, usually side-by-side in tabs.

This guide covers the essential aspects of using both systems effectively. This section contains detailed information about:

- **Managing Software Environments**: How to handle software dependencies and environment modules on each system
- **File and Data Management**: Guidelines for storing, transferring, and managing your research data
- **Job Types and Examples**: Understanding different types of jobs, how to submit them on Calder and Aire, and practical examples for common submission scenarios
- **Job Priority**: Practical explanation of job priority and fair share

Each topic has its own dedicated section with detailed instructions and examples. Use the navigation menu to explore specific topics or follow the sections sequentially for a comprehensive understanding of both Calder and Aire.
