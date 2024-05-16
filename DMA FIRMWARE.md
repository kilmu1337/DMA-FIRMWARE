# Background

In the previous [article](https://github.com/Silverr12/DMA-CFW-Guide/blob/main/Shadow_cfg_space.md), I shared with Silver the method of using shadow cfg space to configure space in a 1:1 ratio. However, many people still ask some questions, such as how to set EXT_CFG_CAP_PTR, what format should be followed in the COE file content, and how to solve the problem of "tiny pcie algo". The following tutorial will explain these details in the steps.



# Requirements

- [vivado](https://www.xilinx.com/support/download.html)
- [Arbor](https://www.mindshare.com/software/Arbor) or [Telescan](https://www.teledynelecroy.com/protocolanalyzer/pci-express/telescan-pe-software/resources/analysis-software)
- [Pcileech-fpga](https://github.com/ufrisk/pcileech-fpga)
- Some basic code foundations, such as being able to use Python for text format conversion.



# Steps

#### 1. open vivdado, build Pcileech-fpga

#### 2. modify the device id and etc.. in the ip core.

Some people may encounter some problems here. I'll give an example: if you only modify the device id vendor id here, then you need to change EXT_CFG_CAP_PTR to 01 later, so that the bar will become chaotic. So I usually modify it all until the end of bar.

#### 3.after modifying the device id in the ip core, choose global instead of out of xxx

#### 4. Remove the is_managed attribute from the IP core and update the EXT_CFG_CAP_PTR and EXT_CFG_XP_CAP_PTR in the pcie_7x_0_core_top.v file.

> [!NOTE]
>
> Note that there is one thing to pay attention to when changing the value here.
>
> In this parameter, each increment of one digit represents a 64-bit extension of the coe coverage, which corresponds to 4 addresses in the configuration space. Therefore, the parameter 01 represents coverage starting from address 0x04, while the parameter 04 represents coverage starting from address 0x10 (in hexadecimal).

#### 5. Modify the contents in cfg space.coe.

Many people make mistakes here because they don't know what the correct format should be converted to. I will provide a corresponding relationship here.

As for the script for converting formats, if needed, I will provide it in future updates.

![img](https://github.com/kilmu1337/DMA-FIRMWARE/blob/main/image.png)

#### 6.After all are completed, the bitstream can be generated.





> [!WARNING]
>
> Some people may encounter errors related to "tiny pcie algo", here I will explain the solution.
>
> First, you need to change the value of Max Payload Size Supported in the corresponding Device Cap in the COE file. I usually modify it to 4kb. Then, in pcie_7x_0_core_top.v, open mps force and change .cfg_force_mps to 4kb.



If there are other questions that are still unclear, please let me know in the issue.
