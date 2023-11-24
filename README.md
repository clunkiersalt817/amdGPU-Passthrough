# amdGPU-Passthrough
iGPU(AMD Radeon Graphics) for ubuntu host and dGPU(AMD Radeon RX 5500m) passthrough for Windows VM

This is a permanent solution and the dGPU is not Dynamically binded
Problem : I experienced many issues while following youtube guides on how to setup a single GPU passthrough or dual GPU passthrough
          1. iGPU and dGPU binding/unbinding using hooks and scripts was very buggy.
          2. when the amdgpu driver module is unloaded both the iGPU and dGPU had to unbinded.
          3. I was not able to use the vfio-pci driver because amdgpu driver was loaded earlier during the boot process.
Solution : I only needed my dGPU to passthrough to Windows VM because you know... to play games, watch movies, etc., on Windows.
          and for other work purposes i need Linux.

          
