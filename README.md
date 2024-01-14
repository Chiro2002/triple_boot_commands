# Tripple Boot Commands
 Any resemblance to sentient operating systems is purely coincidental. If your VM starts grooving or demanding a coffee break, blame its newfound personality – we just provided the stage XD!
 
## Introduction
In this repository, we provide commands to set up a triple boot system with Windows, Solaris, and Ubuntu. The process involves creating virtual disk images for each operating system within VirtualBox.

### Virtual Disk Setup
- Windows Image: Main virtual disk (90GB, utilized 30GB for Windows).
- Solaris Image: Created using an existing disk (Windows VDI, 30GB).
- Ubuntu Image: Located at the same location as the Windows VDI (90GB).

## Instructions after successfully install 3 os
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

After rebooting, open Ubuntu and take a screenshot of GRUB showing all three OS (1grub.png).

df -h   # Take a screenshot (`2df.png`)
sudo fdisk -l

```
### 2. Mounting Windows to /mnt1
Open Ubuntu and run the following commands
```
sudo fdisk -l
sudo mkdir /mnt1   # Create directory
sudo mount /dev/sda2 /mnt1   # Assuming Windows is at sda2

Take a screenshot of the mounted Windows (3windows.png)
sudo ls /mnt1

```

### 3. Solaris Operations
Inside Ubuntu, execute the following commands for Solaris operations.
```
sudo apt-get install zfsutils-linux
lsmod
sudo zpool import -f rpool
sudo ls /rpool
sudo zfs mount rpool/ROOT/opensolaris
df -h
sudo zfs mountpoint=/mnt2 rpool/ROOT/opensolaris
df -h

Take a screenshot of Solaris operations (4thirdos.png).
sudo ls /mnt2

```

### 4. Partition Information
Take screenshots of partition information.
```
cat /proc/partitions   # Screenshot (`5partitions.png`)
df -h

```
### 5. Reboot and Check Windows Partitions
Reboot and open Windows. Search for disk partitions and take a screenshot (6partitions.png).

### 6. Reboot and Check Solaris Partitions
Reboot and open Solaris. Run the following command to see partitions.
```
sudo format

Take a screenshot of Solaris partitions (7partitions.png).
```

This addition emphasizes the importance of conducting personal research before executing the commands provided in the README file.

