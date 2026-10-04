# Compute node types

The Aire and Calder HPC systems provide several types of compute nodes, each designed for different computational workloads. This section provides an overview of the standard CPU nodes, high-memory CPU nodes, and GPU nodes available on each system.

## Standard CPU node

::::{tab-set}

:::{tab-item} Calder

- 96 nodes
- 2 x AMD EPYC 9555 processors, providing 128 CPU cores per node
- 768 GB memory
- 480 GB BOSS-N1 M.2 local storage

:::

:::{tab-item} Aire

- 52 nodes
- Dell R6625 servers
- 2 x AMD EPYC 9634 processors, providing 168 CPU cores per node
- 768 GB DDR5-4800 memory
- 2 x 480 GB M.2 local storage

:::
::::

## High-memory CPU node (Aire only)

Calder does not have dedicated high-memory CPU nodes. Aire provides two high-memory CPU nodes for workloads that require more memory than is available on a standard CPU node.

- 2 nodes
- Dell R6625 servers
- 2 x AMD EPYC 9634 processors, providing 168 CPU cores per node
- 2.3 TB DDR5-4800 memory
- 2 x 480 GB M.2 local storage

## GPU node

::::{tab-set}

:::{tab-item} Calder

- 7 nodes
- 8 x NVIDIA H200 NVL GPUs per node
- 2 x AMD EPYC 9555 processors
- 1.5 TB memory
- 6.4 TB E3.S Gen5 SED local storage
- NVLink 4-way bridges
- **56 GPUs** in total

:::

:::{tab-item} Aire

- 28 nodes
- 3 x NVIDIA L40S 48 GB GPUs per node (PCIe)
- AMD EPYC 9254 processors, providing 24 CPU cores per node
- 256 GB DDR5-4800 memory
- 2 x 480 GB M.2 local storage
- **84 GPUs** in total

:::
::::

## Purchasing additional nodes for Aire

If your project requires additional resources, you may be able to purchase nodes for priority access within the Aire HPC system. Please contact Research IT to discuss your requirements and the available options.

### Guidelines for node purchases

- **Quote details:** The quoted price covers the cost of the hardware. Management and maintenance of the system are provided centrally and are not included in the hardware quote.
- **Data centre capacity:** The number of nodes that can be added depends on available space in the data centre. Please consult Research IT to confirm the current capacity.
- **Node lifespan:** Purchased nodes are managed as part of Aire's infrastructure and will remain available until the system's retirement date of 31/07/2029, regardless of when they are purchased.
- **Access model:** Purchasing a node grants priority access to its resources, not exclusive ownership. This policy maximises resource utilisation and promotes energy efficiency. For specific arrangements, please discuss with Research IT.

### Estimated node costs

| Node type | CPU cores per node | Memory per node | Processor | Estimated cost |
| --- | ---: | ---: | --- | ---: |
| Standard CPU node | 168 | 768 GB | 2 x AMD EPYC 9634 | £39,651.00 + VAT |
| High-memory CPU node | 168 | 2.3 TB | 2 x AMD EPYC 9634 | To be confirmed |
| GPU node | 24 | 256 GB | 3 x NVIDIA L40S | £41,755.00 + VAT |

:::{note}
Please contact Research IT to discuss your specific requirements, obtain a quote, and explore how additional nodes could support your research project. The estimated costs above are indicative and subject to market variation. The final price for any purchased node will be confirmed at the time of purchase.

Estimated node costs for Calder will be provided at a later date.
:::

### Requesting Research Software Engineering support

In addition to infrastructure, we offer Research Software Engineering (RSE) support to help researchers make the most of the available resources. Our RSE team can provide expertise in optimising code, parallelising workflows, and ensuring efficient use of HPC systems. The full consulting catalogue and instructions for requesting our services are available on our main [ARC website](https://arc.leeds.ac.uk/consulting/).
