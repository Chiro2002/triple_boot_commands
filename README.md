# Tripple Boot Commands
Virtual Disclaimer : Any resemblance to sentient operating systems is purely coincidental. If your VM starts grooving or demanding a coffee break, blame its newfound personality – we just provided the stage XD!

Contributions are always welcome here, especially if you've broken a VM a few times—or if your code secretly aspires to be a virtual troublemaker. Embrace the chaos (responsibly)!
 
## Introduction
In this repository, we provide commands to set up a triple boot system with Windows, Solaris, and Ubuntu. The process involves creating virtual disk images for each operating system within VirtualBox.

### Boot Order : 
 In the same order , the installation of the OS'es should be done otherwise it won't be successful
 - Windows
 - Unix System (Open Solaris)
 - Linux System (Ubuntu)

   The reason why this boot order should be followed is because:
   - Windows conisder's whole disk is assigned to it, So it just vanishes the disk and install windows, Windows need's to be installed on primary partition.
   - Unix System(Open solaris) also needs primary partition for installation process.
   - Linux system(Ubuntu) can be installed on primary as well as logical partition , hence it should be installed at last.
     (Note : Primary partition is where OS reside and logical partition is where the data resides)
     
### Virtual Disk Setup
- Windows Image: Main virtual disk (90GB, utilized 30GB for Windows).
- Unix System (Open Solaris) Image: Created using an existing disk (Windows VDI, 30GB).
- Linux System (Ubuntu)  Image: Located at the same location as the Windows VDI (90GB).

## Now catch is , You will see ubuntu and windows in grub menu , Open Solaris lost ??
==> No, It's not lost , just your ubuntu's grub has overriden the previous grub and you need to add open solaris entry in grub.

## Instructions after successfully installed 3 disks images:
Follow the steps below to complete the assignment successfully.

### 1. Adding Solaris to GRUB Menu
Open Ubuntu and add Solaris to the GRUB menu.
```
# See Solaris disk location (e.g., sda3 or sda2)
sudo fdisk -l

# Edit grub.d to add Solaris menu
sudo gedit /etc/grub.d/40_custom

//add below code to 40_custom

menuentry "Solaris" {
  set root='(hd0,3)'   # Use appropriate disk number (e.g., 3 for sda3)
  chainloader +1
}

# Add permissions to run
sudo chmod 777 /etc/grub.d/40_custom
sudo update-grub
reboot

After rebooting, you can view now all 3 OS in Grub menu

```
### 2. Mounting Windows to /mnt1
Open Ubuntu and run the following commands
```
sudo fdisk -l
sudo mkdir /mnt1   # Create directory
sudo mount /dev/sda2 /mnt1   # Assuming Windows is at sda2

sudo ls /mnt1  

```

### 3.  Mounting Solaris to /mnt2
Solaris has zfs file system , which is not compatile to linux by default , so we need to make linux compatible with that file system
Inside Ubuntu, execute the following commands for Solaris operations.
```
sudo apt-get install zfsutils-linux # Installs zfs and zpool
lsmod  #Shows currently loaded modules in the kernal ,if you find zfs in that then only above installation is successful
sudo zpool import -f rpool #rpool is ROOT POOL , which contains file structure of the open Solaris , zpool is used to import the file system to linux via a pool
sudo ls /rpool 
sudo zfs mount rpool/ROOT/opensolaris #Now it mounts the file system to '/' by default
df -h
sudo zfs mountpoint=/mnt2 rpool/ROOT/opensolaris # Now just we change the mount point to /mnt2
df -h

sudo ls /mnt2

```

### 4. Partition Information
Linux System :
```
cat /proc/partitions   
df -h

```
### 5. Reboot and Check Windows Partitions
Reboot and open Windows. Search for disk partitions.

### 6. Reboot and Check Solaris Partitions
Reboot and open Solaris. Run the following command to see partitions.
```
su root  # Now you are superuser
format #select disk as 0
fdisk #Shows you partition information

```

This addition emphasizes the importance of conducting personal research before executing the commands provided in the README file.

