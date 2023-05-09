+++
date = "2023-03-23T23:21:00-05:00"
title = "NixOS and Windows dual system in AN515-55 machine"
[taxonomies]
    categories = ["tutorials"]
    tags = [ "NixOS" ]
+++

> This is a tutorial of install `NixOS` and `Win10` in `AN515-54` computer.

<!-- more -->

#### Reason 

Author has trying to use `NixOS` as default Operating System, and BC the need of using `windows` Operating System, thus i have to install `windows` in my computer, which the `windows` performance in `VirtualBox` is awful and couldn't meet my needs.

#### Current Status

BC authors have been installed `NixOS` in Full disk. The `startup item` and others disk like `MSR`, `rescure` provide by `Acer` is gone.
When trying to install `Win10` after install `NixOS`， it's unable to enter the installation normally just like we did in the new hard drive. Then I was guessing if it's BC some kinds of `bios` setting incorrectly and try to return it into factory settings, but the `bios` stuck --It seems to have the same situation if there is a `arch setup item`.--

#### Computer Information

#TODO

### Step By Step Operation

#### Basic Setup of BIOS

1. BIOS setup 1:
    1. Keeping press the `F2` button after you press the power button to go into the BIOS.
    2. Enable `F12 Boot Menu`, Disable `Fast Boot` and `Secure Boot`
2.  your `Multiboot USB` create by `Ventory` or others. [1: Ventory](https://www.uubyte.com/blog/how-to-use-ventoy-to-create-a-multiboot-usb/) [2: YouTube::Ventory](https://www.youtube.com/watch?v=n8dnTmHMlWU)
3. Install Windows first, I guess the problem when you install `NixOS` then install `win10` will failure due to the startup entry. And by the ways, if you want to install another distro, i thought you should keep enough place for you `linux` in SSD. 

Because the default `sata mode` of this compute is `Optane with RAID`, there may case a problem that the `linux` driver couldn't detect the `SSD` correctly, which you couldn't see the `SSD` driver when you using `sudo fdisk -l` or `gprted`. **You have to change you SATA mode into `AHCI` to enable it**. [3: Intel RST makes SSD disappear in Linux](https://superuser.com/questions/1655079/intel-rst-makes-ssd-disappear-in-linux) And because the windows is installed in `Optane with RAID` mode, and only install the drivers for `Optane with RAID`, so if we change the SATA mode into `AHCI` will case a situation that we couldn't boot the `win10`. We have to use `safe mode` to let `win10` automatically install the `AHCI` drivers.

#### Enable Windows in AHCI SATA mode

1. Open your `win10`
2. Press the Windows logo key + R.
3. Type `msconfig` in the Open box and then select OK. 
4. Select the Boot tab.
5. Under Boot options, choose the Safe boot checkbox and make sure you have chosen the minimal item.
6. Save and Reboot then continuous press the `F2` button to going to the `BIOS::Main`
7. Press `Ctrl + S` at the same times, now you should be able to see the option of `SATA Mode`, change the option into `AHCI`. [5: Switch from RAID to AHCI on Acer Nitro 5 AN515-54](https://community.acer.com/en/discussion/592158/switch-from-raid-to-ahci-on-acer-nitro-5-an515-54) And Save it.
8. Then you should be able to using `win10` in `safe mode`
9. Just repeat the action above In the opposite direction and reboot, then you should be able to boot into windows in `AHCI` mode.

<hr></hr>

It shouldn't have any problem if you installed the `NixOS` from the `Graphical ISO image`. [download link](https://nixos.org/download.html).

#TODO unfinish









### Reference

1. [ How to Use Ventoy to Create a Multiboot USB ](https://www.uubyte.com/blog/how-to-use-ventoy-to-create-a-multiboot-usb/)
2. [ How to Install Ventoy in Linux 2021 Guide ](https://www.youtube.com/watch?v=n8dnTmHMlWU)
3. [ Intel RST makes SSD disappear in Linux ](https://superuser.com/questions/1655079/intel-rst-makes-ssd-disappear-in-linux)
4. [Start your PC in safe mode in Windows](https://support.microsoft.com/en-us/windows/start-your-pc-in-safe-mode-in-windows-92c27cff-db89-8644-1ce4-b3e5e56fe234)
5. [Switch from RAID to AHCI on Acer Nitro 5 AN515-54](https://community.acer.com/en/discussion/592158/switch-from-raid-to-ahci-on-acer-nitro-5-an515-54)

### credibility statement and to-do list  
 
> The following references have not been found or cannot be guaranteed to be correct 

1. Windows couldn't boot up from `AHCI` STAT mode is BC haven't drivers of `AHCI` mode.

> The following list hasn't been explained

1. What is the reason of bios couldn't `load setup defaults`.
