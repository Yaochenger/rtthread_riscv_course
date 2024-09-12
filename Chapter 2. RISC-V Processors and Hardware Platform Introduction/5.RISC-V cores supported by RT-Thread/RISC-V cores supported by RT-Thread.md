### RT-Thread Supported RISC-V Cores

RT-Thread has very rich support for RISC-V MCUs and supports many RISC-V cores and chips Next we introduce the RISC-V cores and chips supported by RT-Thread.

The RISC-V cores supported by RT-Thread are shown in the figure below:

![](figures/riscv_core.png)

The RT-Thread supported RISC-V chips and board support packages are shown below:

<img src="figures/riscv_bsp1.png" style="zoom:80%;" />

![](figures/riscv_bsp2.png)

The above are the RISC-V kernels supported by RT-Thread and the chips using the corresponding kernels. Meanwhile, RT-Thread has adapted a complete software package for these chips, which implements the drivers for most of the peripherals on the board, so that the user can directly use the corresponding BSP and customise his own hardware solution based on the BSP for development.