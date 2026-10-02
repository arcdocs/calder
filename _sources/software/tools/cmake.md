# CMake

[CMake](https://cmake.org/) is a cross platform, open source build system used to control the software compilation process using simple, platform and compiler independent configuration files.

This page provides guidance on the general use of CMake on Calder. For detailed information about CMake and its advanced features, please refer to the [official CMake documentation](https://cmake.org/documentation/).

## CMake on Calder

CMake is not automatically available on Calder. To use CMake, first load the Calder CPU environment and then load the CMake module:

```bash
module load calder/cpu
module load cmake/3.31.11
```

You can check the version of CMake currently loaded with:

```bash
cmake --version
```

The CMake version currently available on Calder is **3.31.11**.

For guidance on building software with CMake on HPC, see the [HPC2 training material](https://arctraining.github.io/hpc2-software/course/cmake.html).
