# System Info
  - This is a Laptop
  - MSI Bravo 15 B5DD
  - CPU - Ryzen 5600H
  - GPU - iGPU (Radeon Graphics), dGPU (Radeon RX 5500M)
  - RAM - 16GB

# amdGPU-Passthrough
- iGPU(AMD Radeon Graphics) for ubuntu host and dGPU(AMD Radeon RX 5500m) passthrough for Windows VM

- This is a permanent solution and the dGPU is not Dynamically binded
- ***Problem* :** I experienced many issues while following youtube guides on how to setup a single GPU passthrough or dual GPU passthrough
  - iGPU and dGPU binding/unbinding using hooks and scripts was very buggy.
  - when the amdgpu driver module is unloaded both the iGPU and dGPU had to unbinded.
  - I was not able to use the vfio-pci driver because amdgpu driver was loaded earlier during the boot process.
- ***Solution* :** I only needed my dGPU to passthrough to Windows VM because you know... to play games, watch movies, etc., on Windows. and for other work purposes i needed Linux.
  - I used this Guide https://wiki.archlinux.org/title/PCI_passthrough_via_OVMF

- I will Specify the commands i Used
- 1. Binding vfio-pci via device ID
  - edit grub `sudo /etc/default/grub`
  - edit this line `GRUB_CMDLINE_LINUX_DEFAULT="amd_iommu=on iommu=pt vfio-pci.ids=1002:7340,1002:ab38"`
  - your vfio-pci.ids might be different
  - after editing the grub update it by `sudo grub-mkconfig -o /boot/grub/grub.cfg`
- 2. Loading vfio-pci early (to ensure that vfio-pci is loaded and binded with our dGPU before it binds with amdgpu driver)
  - I am using dracut(to install run `sudo apt install dracut-core`)
  - Following the same idea, we need to ensure all vfio drivers are in the initramfs.
  - create a file named `10-vfio.conf` `sudo nano /etc/dracut.conf.d/10-vfio.conf`
  - Add this line `force_drivers+=" vfio_pci vfio vfio_iommu_type1 vfio_virqfd"` in the file you created.
  - regenerate the initramfs using `sudo dracut -f --kver  6.2.0-37-generic`

- ***VM Creation* :**
  - Follow any youtube video to create a `win10` VM
  - `important` I did not had to specify my vBIOS in the XML of my win10 VM
  - amd doesn't need vBIOS to be specified
  - After Windows 10 VM installation completed you have to add your PCI Host devices to VM and Start the VM again to install amd radeon PRO drivers that only install drivers for dGPU and amd HDMI audio.
  - Congrats !! You have successfully installed a Ubuntu-host, Windows-VM, passthrough-GPU  
