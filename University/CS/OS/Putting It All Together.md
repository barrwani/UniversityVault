# Putting it All Together
#cs 

### Step 1: Power up

- Registers zeroed out
- Control registers get defaults
- Real mode: 8086
	- No paging
	- 1 MB of physical memory
- Processor hard-wired:
	- Copy firmware from ROM->RAM
	- Jump to known offset
		- Hardcoded entry point where CPU will start executing 


### Step 2: Firmware


Initialize hardware + provide a runtime for the boot process.

UEFI or BIOS.

Core: 
- Real mode -> long mode
	- Long mode is where paging is enabled
- Create an identity-mapped page table
- Create an initial IDT
- Initialize other processor structures
- Set control registers

Peripherals/Devices: Initialize (UEFI has drivers):
- Disk, USB, Display, Mouse, Keyboard, NIC
	- Device Tree initialization (`CONFIGURATION_TABLE`)
	- Mount VFAT partition -> `/EFI`
	- Load + execute bootloader

### Step 3: Boot loader


- Takes a whole decompression stub + compressed Linux (`vmlinuz`) and gets it running
- Decompression stub decompresses compressed Linux

### Step 4: Kernel

Initialize the system:
- Moving away from identity-mapped address space
- Rewrite the interrupt descriptor table (IDT)
- Load + configure device drivers
- Mount the device that will contain `/` (root)
- FSCK run would run here if the filesystem uses it
  
After initialization, kernel forks and `exec init(8)`

### Step 5: `init(8)`

Init: PID1

Most distributions: systemd

Executes as superuser (root)

- Finish initializing the system
	- Network: DHCP
	- Graphics: Set display resolution (GPU)
	- Power management settings
	- Some of the initial services bind to ports `1,...,` (`tcpserve`)
- Launch the login manager
- Relaunch 

### Step 6: Login


