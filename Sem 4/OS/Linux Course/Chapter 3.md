#### The Boot Process
* It is the procedure for initializing the system
* Following is the boot process 
	* **BIOS**
	* **Master Boot Record (MBR) or EFI Partition**
	* **Boot Loader (GRUB)**
	* **Kernel**
	* **initramfs (Initial RAM disk)**
	* **init (parent process)**
	* **Command shell using getty** 
	* **GUI (Wayland or X**)
* **BIOS** 
	* When the computer is turned on, the Basic Input/Output System (BIOS) initializes the hardware, including the screen and keyboard, and tests the main memory. This process is called (POST) Power on Self Test
	* Power ON -> BIOS -> Initialization (POST)
	* BIOS is stored in ROM. Remainder of the boot process is controlled by the OS 
* **MBR, EFI and Boot Loader**
	* After POST is completed, system control passes from the BIOS to the boot loader
	* Boot Loader is stored on the ROM in the boot sector or the EFI partition
	* Information on the date and time and the important peripherals are loaded from the **CMOS** values (CMOS is battery powered memory storage which keeps a track of the date and time even when the system is turned off)
	* When booting Linux, the boot loader is responsible for loading the kernel image and it the initial RAM disk or filesystem (it contains essential files and device drivers needed to start the system)
![[Pasted image 20250315124317.png]]
**Note - ISOLINUX is used for booting from removable media and DAS U-Boot for booting on embedded devices**
* The boot loader loads the kernel of the OS into RAM and passes control to it 
* Kernels are always compressed and the first step it carries out is uncompressing
* After this, it will check and analyze the system hardware and initialize any hardware device drivers built into the kernel
* **Initial RAM Disk**
	* The initramfs filesystem image contains programs and binary files that perform all actions needed to mount the root filesystem
**Note - The boot loader loads both the kernel and the initial RAM based file system (initramfs) into the memory, so it can be used by the kernel directly** 
* When the kernel is loaded into the RAM, it immediately initializes and configures the computer's memory and the hardware attached to the system