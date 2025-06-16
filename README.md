# Linux-Mint-BTRFS-install-tutorial
A tutorial explaining the process of installing Linux mint with BTRFS


## **On this guide, I'll teach you how to clean install mint 22 with BTRFS snapshots. that way, you can rollback in case anything goes wrong, and the best thing, is that snapshots can be accessed from GRUB.**




### Important Note (updated 2025/06): this tutorial was written with UEFI systems in mind. if you are using a legacy BIOS system, please reffer to the comments down below.




***Disclaimer: I expect that at this point, you have created a bootable USB with Ventoy in GPT mode with secure boot support and placed mint 21.3 or 22 ISO on it. If you haven't, get Ventoy here: https://github.com/ventoy/Ventoy***

***For the case of installing without secure boot, disable secure boot (as some GPUs like nvidia, or some wifi cards have issues with it), fast boot, legacy CSM boot mode and TPM.
If you're installing with secure boot enabled, skip to the right part of this guide.***

In theory, this guide should also work with LMDE. further research is required.
======> ***Update: after further testing, this guide also works with LMDE, following the same steps.***

you may like to enable the BTRFS daemon to auto-update grub after making snapshots with timeshift. here's the explanation:
https://github.com/Antynea/grub-btrfs?tab=readme-ov-file#grub-btrfsd-systemd-instructions
-----------------------------------------------------------------------------------------




## Part 1: Installation.
### Installation with secure boot disabled, single boot method:

*I'll be using a VM for this guide.*

To begin, boot into mint live session. I assume that you know your hardware, and the target drive where to install Mint.
once on mint desktop, connect to the internet, then open gparted from mint menu, located on the bottom left of your screen.

with Gparted opened, you'll face this window. from there, choose the right drive to use for mint. on this case, `/dev/nvmen0n1`. Sometimes, it may show as `/dev/sdX` where X is any letter, like `a b c` etc. Just be sure to target the right drive for mint, like on the following shot.

![VirtualBox_Linux Mint_27_07_2024_01_01_48](https://github.com/user-attachments/assets/2f7914bb-ff6f-4504-830f-5c67ec3e2bfa)

for this case, the drive is empty. I assume yours may be with data. regardless, we have to wipe and partition the drive. 

## Warning! Backup your data before proceeding! this will wipe and delete all your data! You've been warned. the Mint devs, team nor me, take responsibility for any data loss if you don't backup.

To initialize/partition the drive, go to the Device menu, and select create partition table like this:

![VirtualBox_Linux Mint_27_07_2024_01_07_45](https://github.com/user-attachments/assets/a8ecae90-b412-4d54-83f3-faae63b15688)

a new window will come up, asking for partition style. by default, it will select msdos (aka MBR). since we are using a modern system, we will use GPT. Do notice Gparted warning, if you haven't backed up your data!!
![VirtualBox_Linux Mint_27_07_2024_01_10_44](https://github.com/user-attachments/assets/f93ed8a6-c862-4d4e-ab39-dec2b6f2127a)

then, once you select GPT, hit Apply button, and Gparted will proceed to wipe/initialize the drive. it should take some seconds, depending of your hardware.

once done, gparted will show an empty drive. we begin to partition.
![VirtualBox_Linux Mint_27_07_2024_01_12_09](https://github.com/user-attachments/assets/c0a0667d-f42f-4130-b568-df0300c8f2da)

to create a partition, hit the + button, just like on the following shot:
![VirtualBox_Linux Mint_27_07_2024_01_12_57](https://github.com/user-attachments/assets/ca0da186-a806-4d68-a0c9-397137523319)

it will show a dialog, on it, we proceed to create a new ESP (EFI system partition), required to boot mint. configure it as following:
![VirtualBox_Linux Mint_27_07_2024_01_16_28](https://github.com/user-attachments/assets/f943c5d5-6af0-4d4c-8e5f-901ee4200bdf)

once ready, hit add. it will look like this:
![VirtualBox_Linux Mint_27_07_2024_01_17_34](https://github.com/user-attachments/assets/d6bd7701-e4eb-438b-ba67-171efe09f871)


Now, we must create the BTRFS root partition, that will contain Mint and your userdata. create a new partition like before, with the following settings:

![VirtualBox_Linux Mint_27_07_2024_01_18_46](https://github.com/user-attachments/assets/76822616-8282-4db9-862f-f3153c667734)


Once again, hit Add. it will look like this:
![VirtualBox_Linux Mint_27_07_2024_01_19_22](https://github.com/user-attachments/assets/28a8b40e-0713-4112-9c8a-db01dd55bb83)

if you're wondering, there's no need for a swap partition, as Mint installer takes care of that later. also, you can use a swap file to enable hibernation if needed + zram.

once ready, hit the checkmark button like this:
![VirtualBox_Linux Mint_27_07_2024_01_20_46](https://github.com/user-attachments/assets/c18ba72d-0e4d-43cd-a62c-8540de1b1d2d)


Gparted will ask if you're sure to proceed like this:

![VirtualBox_Linux Mint_27_07_2024_01_21_28](https://github.com/user-attachments/assets/d0583661-bc25-449c-8e90-3bc3341b9757)

confirm, and Gparted will begin to partition. it will take some time, depending on your specs.

once done, it will show this dialog.
![VirtualBox_Linux Mint_27_07_2024_01_22_21](https://github.com/user-attachments/assets/6b31a86d-f801-424d-9f7b-02c84f076597)

close it, and you'll get this:

![VirtualBox_Linux Mint_27_07_2024_01_23_04](https://github.com/user-attachments/assets/3672a8ec-4278-432b-9f51-f3e243f3ed81)

now, we need to set the flags for the ESP. right click the ESP and add the boot flag like this:

![VirtualBox_Linux Mint_27_07_2024_01_24_16](https://github.com/user-attachments/assets/71988a35-c24f-4a5b-88f6-b72986942248)
![VirtualBox_Linux Mint_27_07_2024_01_24_34](https://github.com/user-attachments/assets/15340069-95a1-4f74-b3d5-f47ead11c9fb)
![VirtualBox_Linux Mint_27_07_2024_01_24_45](https://github.com/user-attachments/assets/6e0ed007-5a84-4f1d-bd81-1b6bb0d3a94c)


then you'll be back on Gparted. now you can exit it as we are done with this part. now we proceed to install Mint. open the installer from the desktop and do the following, adapting it to your needs:
![VirtualBox_Linux Mint_27_07_2024_01_28_14](https://github.com/user-attachments/assets/bed35d51-3869-4e65-95d9-1c2c5f7e2987)
![VirtualBox_Linux Mint_27_07_2024_01_28_53](https://github.com/user-attachments/assets/9478a04c-1cea-471b-b5ad-997c7ba9d714)

Once you reach this part of the installer, select "Something else" to setup the partitions.
![VirtualBox_Linux Mint_27_07_2024_01_30_00](https://github.com/user-attachments/assets/ef68af55-5d5f-4479-aed7-95a1f6e7503c)

you will find yourself with this screen. 

![VirtualBox_Linux Mint_27_07_2024_01_32_06](https://github.com/user-attachments/assets/d28bdb47-bcd4-4e4b-8f03-4de7dd46e1db)

select the ESP partition, hit the change button and setup it like this:
![VirtualBox_Linux Mint_27_07_2024_01_33_40](https://github.com/user-attachments/assets/6a0751a1-5cc5-4a77-96fd-6c7e47b007c6)

then, select the BTRFS partition, and configure it like this:

![VirtualBox_Linux Mint_27_07_2024_01_34_00](https://github.com/user-attachments/assets/368074ad-f89d-4200-90c1-3784b612699f)
![VirtualBox_Linux Mint_27_07_2024_01_34_09](https://github.com/user-attachments/assets/41c7f20f-c8ba-4247-b2b2-f7a6d1174297)
![VirtualBox_Linux Mint_27_07_2024_01_34_18](https://github.com/user-attachments/assets/4bffc5dd-56e3-4648-9449-d0ea02f8a9f7)

once done, install the bootloader to the ESP partition, like on this shot:

![VirtualBox_Linux Mint_27_07_2024_01_34_51](https://github.com/user-attachments/assets/584775ef-54d0-4d48-a5f1-735311b471b9)

we are done partitioning, once you hit install now button, Mint will warn about the changes, and if you're sure to proceed. confirm the changes by pressing continue.

![VirtualBox_Linux Mint_27_07_2024_01_39_58](https://github.com/user-attachments/assets/17f246c7-a2e2-42a7-befa-b16e177f257e)

then, select your timezone:

![VirtualBox_Linux Mint_27_07_2024_01_41_22](https://github.com/user-attachments/assets/c8e6fa03-264a-4b4f-8d52-8ee14428593e)

then, setup an user to your needs. you can choose here if you want to auto login, or, to be asked for your password at boot. personally, I recommend "ask for my password at boot" for security reasons, unless you have reasons to auto login. 

![VirtualBox_Linux Mint_27_07_2024_01_42_52](https://github.com/user-attachments/assets/1f7cae03-6e53-4a67-b42a-40e37971e914)

for more security, you can encrypt your home folder. for the sake of simplicity on this guide, I won't do it. you can research deeper later on your own, or, research then encrypt it now on this step.

once ready, hit continue, the installer will proceed to install Mint.

![VirtualBox_Linux Mint_27_07_2024_01_49_29](https://github.com/user-attachments/assets/173253d7-9b53-4f28-a415-7e691eb85b5d)

Depending of your hardware, the time it will take to install. meanwhile, go for some snacks and mint tea :)
once it's done, the installer will show this:

![VirtualBox_Linux Mint_27_07_2024_02_33_37](https://github.com/user-attachments/assets/a5b06904-049f-497a-9230-b4984b83690e)

we reboot to the new installed system. remove your flash drive when asked.


## Part 2: setting up Timeshift and GRUB snapshots.

once booted into the fresh installed mint, we have to setup Timeshift.
select BTRFS:

![VirtualBox_Linux Mint_27_07_2024_02_42_29](https://github.com/user-attachments/assets/7d6978c4-fadc-49e3-9e94-b0e49725379c)

then select the drive where mint is installed.

![VirtualBox_Linux Mint_27_07_2024_02_42_38](https://github.com/user-attachments/assets/d567b5f4-5244-47c9-85d8-ebe2977dc377)

now select the frequency of snapshots, I recommend monthly automatic snapshots, but you can also do weekly ones. 

![VirtualBox_Linux Mint_27_07_2024_02_42_48](https://github.com/user-attachments/assets/e41c2c82-2191-4463-8e5e-11fb9a25b66a)

optionally, you can include your home subvolume/home folder on the snapshots.

![VirtualBox_Linux Mint_27_07_2024_02_43_01](https://github.com/user-attachments/assets/f30374a8-8b6f-478e-a04d-14bf6e622618)


once done, hit finish.

![VirtualBox_Linux Mint_27_07_2024_02_43_10](https://github.com/user-attachments/assets/fe8a98fa-015d-4364-b04e-a108c8c26541)


now hit create, to create a snapshot. it will take a second.

![VirtualBox_Linux Mint_27_07_2024_02_43_26](https://github.com/user-attachments/assets/28438be0-8e57-4b23-ac4b-b3216b8f4140)

it will look like this. you can exit Timeshift now.

![VirtualBox_Linux Mint_27_07_2024_03_04_00](https://github.com/user-attachments/assets/bde2b692-267f-4412-9f7c-296d1abd2466)


## Part 3: Setting up snapshots to show up on GRUB.

To make grub show the snapshots, we need an extra package from a github repo, as it's not available on mint repos with APT. for such, we browse to this repo:

https://github.com/Antynea/grub-btrfs

then, we get the latest package from the releases.

![image](https://github.com/user-attachments/assets/94e076e6-8664-45c6-b070-7cf0eac804f6)

we download the tar.gz code.

![image](https://github.com/user-attachments/assets/5638868c-3172-42ac-bd64-e59dcbb7987c)

Once downloaded, we extract it to a folder and open it. it will look like this:

![VirtualBox_Linux Mint_27_07_2024_03_12_34](https://github.com/user-attachments/assets/6b60b119-4f19-431e-9cbd-77ed8aa0b2a7)

then, right click on an empty area of the folder, and open a terminal like this:

![VirtualBox_Linux Mint_27_07_2024_03_12_44](https://github.com/user-attachments/assets/0f429130-5848-4404-82e3-0036123cb295)


![VirtualBox_Linux Mint_27_07_2024_03_13_12](https://github.com/user-attachments/assets/2287bbb2-bab9-4f62-8043-05c2120b5551)

then, run `ls` to show the files of the folder in terminal.

![VirtualBox_Linux Mint_27_07_2024_03_13_19](https://github.com/user-attachments/assets/740f5150-8464-422d-84fb-61b3eea53552)


Now, we proceed to install it. for that, we run:

`sudo apt install gawk && sudo make install`
Enter your sudo password to proceed.
it will pull the dependencies then begin to build and install like this:

![VirtualBox_Linux Mint_27_07_2024_03_18_14](https://github.com/user-attachments/assets/e2276389-0803-42a4-817a-52b961e584b5)
![VirtualBox_Linux Mint_27_07_2024_03_18_32](https://github.com/user-attachments/assets/357d3083-75a5-4946-8a63-eb50d80cc75f)

it will automatically detect your snapshots and add them to GRUB. once done, run `sudo update-grub`. after that, you can exit terminal. now, time to check GRUB.

![VirtualBox_Linux Mint_27_07_2024_03_21_48](https://github.com/user-attachments/assets/d536bd78-f06d-4598-a052-30cf61b31c85)

when booting, you'll be greeted by grub. your snapshots should show up like this:

![VirtualBox_Linux Mint_27_07_2024_03_24_52](https://github.com/user-attachments/assets/92f2a94e-9e20-4e37-b2dd-7957bd3fc129)

you can notice the snapshots menu entry. 

![VirtualBox_Linux Mint_27_07_2024_03_31_03](https://github.com/user-attachments/assets/d445a573-f4ab-42be-8371-0528023a88d7)


## Congratulations! You finished setting up BTRFS.


Now, just make as many snapshots you need, then update grub with `sudo update-grub` and they will show at boot. to rollback, just hit enter on the snapshot on your GRUB menu and it will rollback.  

**If your system updates, or you update the kernel, it may trigger `sudo update-grub`, and therefore, add your newer snapshots to GRUB.** 



## Installation with secure boot disabled, dual boot method, single drive:

Most of this method is similar to the last one, but with some tweaks. I recommend shrinking your Windows main partition from within Windows with either disk management or a 3rd party tool for Windows, then leave unallocated space after it. it should look like this:


![VirtualBox_LSW_28_07_2024_03_06_38](https://github.com/user-attachments/assets/b0ff58c8-965e-42c6-953f-250179811777)

then, just like on the previous method, we make the ESP and BTRFS partitions for mint like this:

![VirtualBox_LSW_28_07_2024_03_07_08](https://github.com/user-attachments/assets/e238961e-40d1-4eec-9e34-40ee4fd24fc2)
![VirtualBox_LSW_28_07_2024_03_07_19](https://github.com/user-attachments/assets/a9ee9c4c-3b86-4052-a629-1edede02f2f8)
![VirtualBox_LSW_28_07_2024_03_07_43](https://github.com/user-attachments/assets/e30a7214-db2b-4260-b994-e52e7ee4c70a)
![VirtualBox_LSW_28_07_2024_03_09_35](https://github.com/user-attachments/assets/a9a6833d-9260-4b68-9a39-378d0e4116c3)
![VirtualBox_LSW_28_07_2024_03_08_51](https://github.com/user-attachments/assets/37cce702-5f14-4c52-897d-a55e6c8feb76)
![VirtualBox_LSW_28_07_2024_03_09_51](https://github.com/user-attachments/assets/0a0bb818-6e30-4c39-b7c1-b3f5ebcb6f7d)
![VirtualBox_LSW_28_07_2024_03_09_01](https://github.com/user-attachments/assets/7fc40a08-8e24-41ed-9180-75a5633579bd)
![VirtualBox_LSW_28_07_2024_03_10_07](https://github.com/user-attachments/assets/c805523b-7fee-4479-b1f1-bdd44dae826b)
![VirtualBox_LSW_28_07_2024_03_54_31](https://github.com/user-attachments/assets/92fe0703-d89e-4f69-8a78-b88bc639593b)
![VirtualBox_LSW_28_07_2024_03_54_36](https://github.com/user-attachments/assets/15777c28-0149-4d2d-a884-87a79f9e80d6)
![VirtualBox_LSW_28_07_2024_03_54_45](https://github.com/user-attachments/assets/ee9136dc-7429-4315-a9c2-10d8734d7a0c)


Then, we proceed to prepare the partitions with Mint Installer like on the previous method:

![VirtualBox_LSW_28_07_2024_04_04_44](https://github.com/user-attachments/assets/a5cea5d9-3d53-41e8-a89a-35150cc80a0d)
![VirtualBox_LSW_28_07_2024_04_06_34](https://github.com/user-attachments/assets/69006380-a42a-4ac4-88c9-eeaa38c5d055)
![VirtualBox_LSW_28_07_2024_04_06_48](https://github.com/user-attachments/assets/a77bf912-74da-4540-b2bf-cbaf897a971a)
![VirtualBox_LSW_28_07_2024_04_06_53](https://github.com/user-attachments/assets/eff80f10-d382-4e9f-b6d0-b1badb12b451)
![VirtualBox_LSW_28_07_2024_04_07_25](https://github.com/user-attachments/assets/ac215abf-fd7e-490c-abd3-a2a0d06a31a0)
![VirtualBox_LSW_28_07_2024_04_07_12](https://github.com/user-attachments/assets/df55219e-2abc-4de6-ae2b-6bb901881510)
![VirtualBox_LSW_28_07_2024_04_07_37](https://github.com/user-attachments/assets/43b6e9b0-6319-46cb-bc30-33f1bcde8dc1)
![VirtualBox_LSW_28_07_2024_04_07_17](https://github.com/user-attachments/assets/b4aa3ea7-779d-4c95-9d13-6bff1c8af8f3)
![VirtualBox_LSW_28_07_2024_04_08_20](https://github.com/user-attachments/assets/8c553fea-a143-45a8-acb8-bac9aa55f89a)
![VirtualBox_LSW_28_07_2024_04_10_18](https://github.com/user-attachments/assets/6fd50248-4631-47ad-b365-7f5e90983e36)

**From here, the installation process is the same as before**
------

Once mint is installed, reboot like on the previous method and proceed to setup Timeshift and GRUB snapshots the same way. the only difference is that GRUB will detect your Windows partition and add it to the list.

## Important note: by making a separated ESP, you avoid Windows from nuking it after an update. 

**for best results, install a dual boot on dedicated drives.**


For some reason, Mint will use your Windows ESP for itself, even if you make a dedicated one. I'm still researching why it does that, but it's the big reason to use a dedicated drive for mint to dual boot.



## Installation with secure boot disabled, dual boot method, dedicated drives:

Same as last method, but instead of shrinking and partitioning on same drive, you target a dedicated drive, and use 1st installation method as reference, then repeat the setup.


## Installation with secure boot enabled, single boot:

Same method as with secure boot disabled and single boot, but at some point you'll encounter this:

![VirtualBox_Mint_28_07_2024_22_46_28](https://github.com/user-attachments/assets/bc40eece-bc34-4073-8adb-afd0152befb5)


where you'll need to enter a password for secure boot. 

The rest of the process is the same as before, regarding partitioning:

![VirtualBox_Mint_28_07_2024_22_49_15](https://github.com/user-attachments/assets/ebb6131d-7d74-4896-bcdf-65fd44d1c77a)

Then you just complete the setup and installation of the GRUB btrfs snapshots and timeshift after a reboot.


## Installation with secure boot enabled, dual boot method, dedicated drives:

It should be same as with single boot, but this time, you target the drive that will contain Mint, while keeping your Windows drive attached. Mint GRUB should add Windows to it's menu at reboot.  Then you proceed to setup Timeshift and GRUB snapshots.


------

### this guide will be updated constantly to cover most users use case.


### Note: If I forget an user case, lemme know in the comments.




## Footnotes:

# - Why I'm writing this guide? 

On my time on the Linux Mint discord server, I came across countless broken Mint installs because the users installed another desktop like GNOME or KDE Plasma which are not supported, then wanted to remove it, and that lead to a broken system, frankenmint, dependency hell or TTY-only systems, some times, unable to recover even with rsync timeshift. So, I began to research the posibility of using BTRFS with mint, as I've already experimented with it on Arch Linux, and so far, it has worked like a charm. now considering the nature of mint as a stable distro, this should come handy for several users, so they can recover easily. 


- How you can contribute?
By testing and giving feedback, so this guide can be updated and improved.

## - Important Note:
Your GRUB will not look the same as mine, but a black screen with white text by default. if you're curious, I used a GRUB theme on the VM, for the sake of making it more legible and pleasant to the eyes.


## - Personal Opinion.

For the future, I would like Mint team to consider using BTRFS as default, or like how EndeavourOS team does, asking the user if they want the classic and traditional ext4, or BTRFS for their installation. 


--------------------------------------------------------------------------------------

**Aknowledgements**: 

Mint team, for creating such a wonderful distro, making Linux easier to use.
Mint discord server, for being with me for years, I wouldn't wrote this if it were not of the issues there.
Antynea, for making grub-btrfs, without it, this would be incomplete.

