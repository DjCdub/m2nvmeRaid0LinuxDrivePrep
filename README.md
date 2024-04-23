# m2nvmeRaid0LinuxDrivePrep
how to setup and format drives for a clean linux install
# ***Step 1: Pre-Installation Preparation***
### Backup Data: Ensure all important data is backed up, as this process will erase existing data.
#### Boot into Live Ubuntu Environment: Use a Live USB or DVD containing the Ubuntu installer.
**Open Terminal: Access the terminal in the live environment.**
# ***Step 2: RAID Setup***
#### **Install mdadm (if not installed):**
```shell
command sudo apt update
command sudo apt install mdadm
```
**Remove Existing RAID Configuration (if necessary):**
```Shell
command sudo mdadm --stop /dev/md0
command sudo mdadm --remove /dev/md0
```
##**Partition Drives:**
####Use fdisk or gdisk to partition each drive according to your specifications.
###Create RAID array:
```Shell
command sudo mdadm --create --verbose /dev/md0 --level=stripe --raid-devices=2 /dev/nvme0n1p[X] /dev/nvme1n1p[X]
```
**(Replace ' [X] ' with the appropriate partition numbers)**
#**Step 3: Partitioning RAID Array**
###Create Partitions:
```Shell
sudo fdisk /dev/md0
```
###My partitioning scheme:
blank space                                      0-15
ESP:        Type: EFI System       (EF00), Size: 0-1023 MiB
/EFI/boot:  Type: Linux filesystem (8300), Size: 1024-2047 MiB
/:          Type: Linux filesystem (8300), Size: 2048-491519 MiB
Swap:       Type: Linux swap       (8200), Size: 491520-524288 MiB
##Format Partitions:
```shell
command sudo mkfs.fat -F32 /dev/md0p1
command sudo mkfs.ext4 /dev/md0p2
command sudo mkfs.ext4 /dev/md0p3
command sudo mkswap /dev/md0p4
```
#**Step 4: Mount Partitions**
##Mount Partitions:
```shell
command sudo mkdir /mnt/esp
command sudo mount /dev/md0p1 /mnt/esp
'''
command sudo mkdir /mnt/efi_boot
command sudo mount /dev/md0p2 /mnt/efi_boot

command sudo mkdir /mnt/root
command sudo mount /dev/md0p3 /mnt/root
#**Step 5: Installation**
Install Ubuntu: or whichever OS
##*Choose "Something else" during installation.*
#**Assign partitions to mount points:**
```
/mnt/esp for ESP
/mnt/efi_boot for /EFI/boot
/mnt/root for root (/)
```
###Complete installation process.
#**Step 6: Post-Installation**
##Configure Boot Loader:
#Ensure GRUB is installed on the RAID array (/dev/md0).
##Update GRUB configuration if necessary.
##Reboot: Restart the system and ensure it boots from the RAID array.
**Following these steps should result in a properly configured RAID array with multiple partitions for installing Ubuntu. Adjust partition sizes and other settings as needed for your specific requirements.**
