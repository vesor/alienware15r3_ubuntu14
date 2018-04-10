# How to install Ubuntu 16.04 on Alienware 15 R3

Hardware: Gsync Display, SSD+HDD, Nvidia GeForce 1070.

When press Fn+F7, it says "not support for Gsync LCD panel". It means it can't use integrated GPU for gsync display.

## Update windows 10 
Start windows.

You should get a BIOS update alert from the Alienware Update widget. If not,
right click on the Down arrow icon in the bottom right extra icons `^` thing and 
right click, then click `Check for Updates`.

Install it. It will reboot your computer, try to not touch anything up until
you are back to Windows.

Better to reboot several times before starting to do the following steps. So that we can let windows get updated.

## Shrink the disk
Go to the disk manager (right click on Windows icon > Disk management) and
Shrink the OS partition (right click on it, `Shrink Volume...`). 
Disable system protection on disk C, otherwise you can't shrink too much. 
My C disk is 64GB after shrink, hope it would be enough for some basic windows apps.

## Get Windows to boot on AHCI mode (SATA options)
For Ubuntu to see the NVME disk it needs to boot on AHCI not on RAID mode (the default).
As you don't want to go to the BIOS to change the SATA mode every time you want to boot
in one or another OS, we need to force Windows to be able to boot in AHCI mode.

For that follow the instructions (kindly taken from [here](http://www.tenforums.com/drivers-hardware/15006-attn-ssd-owners-enabling-ahci-mode-after-windows-10-installation.html)):

1. Run Command Prompt as Admin.
2. Invoke a Safe Mode boot with the command: `bcdedit /set {current} safeboot minimal`.
3. Restart the PC and enter your BIOS during bootup (F2 key).
4. In tab `Advanced` change option `SATA Operation` from `RAID on` to `AHCI` mode then go to `Exit` tab and use `Save Changes and Reset`.
5. Windows 10 will launch in Safe Mode.
6. Right click the Window icon and select to run the Command Prompt in Admin mode from among the various options.
7. Cancel Safe Mode booting with the command: `bcdedit /deletevalue {current} safeboot`.
8. Restart your PC once more and this time it will boot up normally but with AHCI mode activated.
9. Enjoy your awesomeness.


## Disable Secure boot
You need to disable secure boot in order to boot any other OS.

Enter your BIOS (F2 key on boot). Go to `Boot` tab and change `Secure Boot` option to `Disabled`.

Note that `Boot List Option` should be `UEFI` (it's the default).

## Get the latest Ubuntu 16.04 and write it to a bootable pendrive
I ommit this part, I think there are many ways to do that.
I just use a pendrive with ubuntu 16.04 which I used to do backup & restore.

## Boot from the live linux pendrive
Press F12 while booting and choose under `UEFI OPTIONS` to boot from your pendrive, for me it was
`USB1 - UEFI OS( USB DISK 3.0 PMAP)`.

## Use the install Wizard
Just double click the `Install Ubuntu 14.04.05 LTS` desktop icon.

Configure as you like BUT DON'T ENABLE DOWNLOAD UPDATES WHILE INSTALLING NOR INSTALL THIRD PARTY SOFTWARE. It will freeze your installation. If you don't believe me, just try and enjoy your reboot.

I choose the first option instead of `Something else`.
The ubuntu installer will automatic install system and boot loader to `/dev/nvme0n1`.

Now you can click `Install Now`.

In a few minutes you should be good to go!

Remember to format HDD to ext4, it should be NTFS by default.

# How to use only the nvidia 1070 GTX card on Alienware 15 R3
When first time reboot into the newly installed ubuntu, you'll need to modify the boot params when you see the grub menu.
Add just before `quiet splash` the word `nomodeset` so it will look like:
`nomodeset quiet splash`. Then press F10 to boot into it.

I use wired network, and it seems work fine in ubuntu.
I use sudo apt update & apt upgrade. Then reboot. (This may not be necessary, but I just did it.)

# Install CUDA

wget "http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1604/x86_64/cuda-repo-ubuntu1604_8.0.44-1_amd64.deb" -O "cuda-repo-ubuntu1604_8.0.44-1_amd64.deb"

sudo dpkg -i cuda-repo-ubuntu1604_8.0.44-1_amd64.deb
sudo apt-get update
sudo apt-get -y install cuda
sudo modprobe nvidia
nvidia-smi

