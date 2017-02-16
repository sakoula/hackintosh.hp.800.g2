**Installing and configuring macOS on a 'HP EliteDesk 800 G2 TWR'**

Table of Contents
=================

* [categories: hackintosh elcapitan sierra bog](#categories-hackintosh-elcapitan-sierra-bog)
 * [Software Repository](#software-repository)
 * [Hardware Specs](#hardware-specs)
 * [Windows Setup](#windows-setup)
    * [Install Windows](#install-windows)
    * [Drivers and BIOS](#drivers-and-bios)
    * [Configure BIOS Settings](#configure-bios-settings)
    * [Benchmarking Windows 10](#benchmarking-windows-10)
 * [Install using Multibeast](#install-using-multibeast)
    * [Prepate USB stick](#prepate-usb-stick)
    * [Use Unibeast](#use-unibeast)
       * [Clover config.plist](#clover-configplist)
       * [Clover Configuration](#clover-configuration)
       * [Clover Kexts](#clover-kexts)
    * [Install Sierra 10.12.2](#install-sierra-10122)
       * [Install OSX](#install-osx)
       * [Run Multibeast](#run-multibeast)
       * [Inspect configuration](#inspect-configuration)
    * [Additional tweaks](#additional-tweaks)
       * [HFSPlus for VBoxHfs-64](#hfsplus-for-vboxhfs-64)
       * [USB clover patch](#usb-clover-patch)
       * [SSDT gen](#ssdt-gen)
       * [VoodooHDA glitch](#voodoohda-glitch)
       * [Video artifacts](#video-artifacts)
       * [Video second monitor](#video-second-monitor)
       * [Video second monitor &gt;= 10.12.3](#video-second-monitor--10123)
       * [Video Performance](#video-performance)
       * [Install RC scripts on target volume](#install-rc-scripts-on-target-volume)
       * [BIOS time <g-emoji alias="gun" fallback-src="https://assets-cdn.github.com/images/icons/emoji/unicode/1f52b.png" ios-version="6.0">🔫</g-emoji>](#bios-time-gun)
       * [System Preferences &gt; Energy Saver](#system-preferences--energy-saver)
       * [Reboots on Shutdown](#reboots-on-shutdown)
       * [Disk Renumbering](#disk-renumbering)
       * [DSDT](#dsdt)
 * [Install 10.12 my way manually](#install-1012-my-way-manually)
    * [Prepare USB stick](#prepare-usb-stick)
    * [Install Clover USB stick](#install-clover-usb-stick)
    * [Install Sierra 10.12.2](#install-sierra-10122-1)
    * [Configure Sierra 10.12.2](#configure-sierra-10122)
    * [System Preferences Initial Configuration](#system-preferences-initial-configuration)
 * [Benchmarking Sierra 10.12.2](#benchmarking-sierra-10122)
 * [HDD/SSD performance](#hddssd-performance)
 * [Upgrade to 10.12.3](#upgrade-to-10123)
 * [References](#references)
 * [Software](#software)

Created partially by [gh-md-toc](https://github.com/ekalinin/github-markdown-toc)

### Hardware Specs ###
[up up up](#)

* Product Number: L1G77AV
* HP EliteDesk 800 G2 Tower PC

hardware configuration with the following specs:

* HP EliteDesk 800 G2 TWR
* Intel® 8 Q170
* MOBO HP 8053 (U3E1)
* Intel Core i7 6700 @ 3.40GHz Skylake
* 32GB DDR4 2133 DIMM (Dual-Channel Unknown @ 1064MHz (15-15-15-36))
* GPU: Integrated: Intel® HD Graphics 530 with (2) DisplayPorts with multi - stream and VGA
* audio: Realtek ALC 221 Audio
* network: Intel® I219LM Gigabit Network Connection LOM
* 238GB SanDisk SD7SB3Q-256G-1006 (SSD)   35 °C
* 931GB Seagate ST1000DM003-1SB102 (SATA) 31 °C

full specs from the [HP site](http://store.hp.com/us/en/pdp/hp-elitedesk-800-g2-tower-pc-p-w5x93ut-aba--1)

### Windows Setup ###

#### Install Windows ####
[up up up](#)

Use [Rufus](https://rufus.akeo.ie/) to create a bootable USB stick with windows10 enterprise 64-bit

#### Drivers and BIOS ####
[up up up](#)

Install on one disk Windows 10 and make sure that you check with this [link](http://support.hp.com/us-en/product/hp-elitedesk-800-g2-tower-pc/7633297/drivers) to update drivers and BIOS.

installed:

* `sp72786`: Intel Chipset support for windows 10.1.1.9
* `sp72957`: Realtek High Definition Audio Driver 6.0.1.7503
* `sp78219`: Intel Management Engine Driver 11.5.0.1019
* `sp78292`: BIOS 00.02.17 (file:///C:/SWSETUP/SP78318/ME%20Update.htm)
* `sp78318`: Desktop ME Firmware 11.0.18.1002 (file:///C:/SWSetup/SP78292/BIOS%20Flash.htm)
* `sp78416`: Intel Graphics Driver 21.20.16.4528 (HD 530)
* `sp77049`: Intel I219 NIC 21.0_12.15.23.1

#### Configure BIOS Settings ####
[up up up](#)

* ~~Broke Raid without any problem on booting from one disk~~ (computer came with no raid)
* ~~Setup disk as AHCI~~ (there is no option on the BIOS)
* `Advanced > Security > TPM Embedded Security > TPM Device` **Hidden**
* `Advanced > Security > BIOS SureStart >` **Unchecked ALL**
* `Advanced > Security > System Management Command` **Unchecked**
* `Advanced > Boot Options > UEFI boot order` **Checked** (do this or else it will not boot from the stick)
* `Advanced > Secure Boot Configuration >` **Unchecked ALL**
* `Advanced > Secure Boot Configuration > Configure Legacy Support and Secure Boot` **Legacy Support Enable and Secure Boot Disable**
* `Advanced > System Options > Configure Storage Controller for RAID` **Unchecked**
* `Advanced > System Options > VTx` **Checked**
* `Advanced > System Options > VTd` **Unchecked**
* `Advanced > System Options > PCI*` **Unchecked**
* `Advanced > Built-In Device Options > Video memory size` **512MB** [here](https://www.tonymacx86.com/threads/skylake-intel-hd-530-integrated-graphics-working-as-of-10-11-4.188891/page-40) (I used the maximum since I have enough RAM on the machine)
* `Advanced > Port Options > Serial Port A` **Unchecked**
* `Advanced > Port Options > Rest` **Checked**
* `Advanced > Power Management Options > Runtime Power Management` **Checked**
* `Advanced > Power Management Options > Extended Idle Power States` **Checked**
* `Advanced > Power Management Options > S5 Maximum Power Savings` **Checked**
* `Advanced > Power Management Options > SATA Power Management` **Checked**
* `Advanced > Power Management Options > PCI Express Power Management` **Checked**
* `Advanced > Power Management Options > Power On from Keyboard Ports` **Unchecked**
* `Advanced > Power Management Options > Unique Sleep State Blink Rates` **Unchecked**
* `Advanced > Remote Management Options > Active Management (AMT)` **Unchecked**

#### Benchmarking Windows 10 ####
[up up up](#)

* `GeekBench x64 4.0.3 CPU` 4796/15263
* `GeekBench x64 4.0.3 CPU` 4787/15225
* `GeekBench x64 4.0.3 GPU/OpenCl` 21496
* `GeekBench x64 4.0.3 GPU/OpenCl` 21479
* `CINEBENCH R15.038_RC184115 OpenGL` 55.71fps
* `CINEBENCH R15.038_RC184115 OpenGL` 55.53fps
* `CINEBENCH R15.038_RC184115 CPU` 824cb
* `CINEBENCH R15.038_RC184115 CPU` 821cb
* `LuxMark-v3.1 OpenCL GPU` 2544
* `LuxMark-v3.1 OpenCL GPU` 2541
* `LuxMark-v3.1 OpenCL CPU` 2253
* `LuxMark-v3.1 OpenCL CPU` 2274
* `LuxMark-v3.1 Native C++` 2569
* `LuxMark-v3.1 Native C++` 2532

### Install using Multibeast ###
[up up up](#)

Initially I installed the machine using multibeast. I wanted to get it up fast to check the hardware under OSX.

#### Prepate USB stick ####
[up up up](#)

https://www.tonymacx86.com/threads/unibeast-install-macos-sierra-on-any-supported-intel-based-pc.200564/

format usb stick:

* GUID Partition Table
* Name: USB
* Format: MacOS Extended (Journaled)

#### Use Unibeast ####
[up up up](#)

download and run `Unibeast (7.0.1)` which includes `Clover EFI v2.3 r3766`

select `UEFI Boot Mode` and create an installation **USB Stick**. 

I examined the configuration on the **USB stick** and it is the following:

##### Clover `config.plist` #####

* `Boot > darkware`
* `Boot > dart=0` # this disabled the Vtd if enabled on OSX
* `Boot > nv_disable=1`
* `Graphics > Inject Intel`
* `Kernel And Kext Patches > Apple RTC`
* `Kernel And Kext Patches > Asus AICPUPM`
* `Kernel And Kext Patches > KernelPm`
* `Kernel And Kext Patches > KextsToPatch > AppleAHCIPort`

```xml
    <dict>
        <key>Comment</key>
        <string>External icons patch</string>
        <key>Find</key>
        <data>
        RXh0ZXJuYWw=
        </data>
        <key>Name</key>
        <string>AppleAHCIPort</string>
        <key>Replace</key>
        <data>
        SW50ZXJuYWw=
        </data>
    </dict>
```

* `Kernel And Kext Patches > KextsToPatch > AppleUSBXHCIPCI`

```xml
    <dict>
        <key>Comment</key>
        <string>change 15 port limit to 26 in XHCI kext (100-Series-10.12)</string>
        <key>Find</key>
        <data>
        g710////EA==
        </data>
        <key>Name</key>
        <string>AppleUSBXHCIPCI</string>
        <key>Replace</key>
        <data>
        g710////Gw==
        </data>
    </dict>
```

* `Rt Variables > BooterConfig > 0x28`
* `Rt Variables > CsrActiveConfig > 0x3`
* `SMBIOS > Product Name > iMac14.2`
* `System Parameters > Inject System ID`

Just in case you want to know what `CsrActiveConfig` does, this is taken from the clover wiki:

```text
Relevant user options for SIP are as follows:
BooterConfig=0x28
csr-active-config 0x0 = SIP Enabled (Default)
csr-active-config 0x3 = SIP Partially Disabled (Loads unsigned kexts)
csr-active-config 0x67 = SIP Disabled completely
```

and a note why we are using `dart=0` and what about `VT-d`

```text
dart=0
Disables the VT-d virtualization technology built into certain Intel processors. For Hackintoshes, VT-d is pretty useless; virtually no Mac OS X applications use it (virtualization apps like Virtualbox tend to use the alternative VT-x technology instead), and certain Hackintosh motherboards have been known to crash in Mac OS X when VT-d is enabled.
```

##### Clover Configuration #####

* `Install for UEFI booting only`
* `Install Clover in the ESP`
* `Drivers64UEFI > OsxAptioFix2Drv-64`

##### Clover Kexts #####

The following kexts have been installed in `EFI/CLOVER/kexts/10.11`:

* `AppleIntelE1000e.kext`
* `AtherosE2200Ethernet.kext`
* `FakeSMC.kext`
* `RealtekRTL8111.kext`
* `USBInjectAll.kext`

#### Install Sierra 10.12.2 ####
[up up up](#)

##### Install OSX #####

unplug everything but two disk / keyboard / wired mouse.

To install you can use either the VGA port or one of the DP ports. When using the VGA port obviously you will not have any acceleration and in order to avoid having a black screen on the computer you always need to uncheck the `Inject Intel` on the `config.plist`.

Install Sierra using any port. I used a USB3 port on the back. (`USBInjectAll.kext` is doing a great job with the Intel based USB ports)

On the installer open disk utility and format the boot disk as `GUID Partition Map`/`Macintosh HD`/`Mac OS Extended (Journaled)`

Install OSX. 

> Have in mind that in order to boot OSX directly from the installed partition you need to have clover installed on the partition. During the installation procedure and on the first time that you boot on your freshly installed OS you need to boot from the **USB Stick** and select the disk partition that you have installed OSX.

##### Run Multibeast #####

Finally boot using the **USB Stick** on OSX, run the latest multibeast and select:

* `Quick Start > UEFI Boot Mode`
* `Drivers > Audio > Universal > VoodooHDA v2.8.8`
* `Drivers > Misc > FakeSMC*`
* `Drivers > Network > IntelMausiEthernet v2.2.0` **AppleItelE1000e does not work for this mobo**
* `Drivers > USB > Increase Max Port Limit` (USBInjectAll.kext)
* `Bootloaders > Clover UEFI Boot Mode`
* `Customize > Intel HD 530` 

```text
(Fix for most Intel HD 530 graphics port layouts on 100 Series motherboards. Adds 4 display patch for AppleIntelSKLGraphicsFramebuffer to `/Volumes/EFI/EFI/CLOVER/config.plist`. Adds `ig-platform-id=0x19120000`to `/Volumes/EFI/EFI/CLOVER/config.plist`.)
```

* `Customize > System Definitions > iMac17,1` (change from the default which is iMac14,2)

> **Note on system definition** : Apparently there is an *i7 6700 @ 3.40GHz* on an iMac which is [this](http://www.everymac.com/systems/apple/imac/specs/imac-core-i7-4.0-27-inch-aluminum-retina-5k-late-2015-specs.html). Since that one has system definition *iMac17.1* I choose this.

> **Notice** : MultiBeast installs all kexts in `/Library/Extensions`. When tested Voodoo did not had correct permissions, and had to fix manually

##### Inspect configuration #####

**config.plist**

* `ACPI > Generate PStates` **had to add it manually to fix speedstepping**
* `ACPI > Generate CStates` **had to add it manually to fix speedstepping**
* `Boot > verbose`
* `Boot > darkware`
* `Boot > dart=0`
* `Graphics > ig-platform-id > 0x19120000`
* `Graphics > Inject Intel`
* `Kernel And Kext Patches > Apple RTC`
* `Kernel And Kext Patches > Asus AICPUPM`
* `Kernel And Kext Patches > KernelPm`
* `Kernel And Kext Patches > KextsToPatch > AppleUSBXHCIPCI`

```xml
      <dict>
        <key>Comment</key>
        <string>Raise 15 port limit to 30 in AppleUSBXHCIPCI</string>
        <key>Find</key>
        <data>
        g72M/v//EA==
        </data>
        <key>Name</key>
        <string>AppleUSBXHCIPCI</string>
        <key>Replace</key>
        <data>
        g72M/v//Hw==
        </data>
      </dict>
```

* `Kernel And Kext Patches > KextsToPatch > AppleAHCIPort`

```xml
      <dict>
        <key>Comment</key>
        <string>External icons patch</string>
        <key>Find</key>
        <data>
        RXh0ZXJuYWw=
        </data>
        <key>Name</key>
        <string>AppleAHCIPort</string>
        <key>Replace</key>
        <data>
        SW50ZXJuYWw=
        </data>
      </dict>
```

* `Kernel And Kext Patches > KextsToPatch > 10.11-SKL-1912000-4_displays`

```xml
<dict>                 
    <key>Comment</key>
    <string>10.11-SKL-1912000-4_displays</string>
    <key>Find</key>
    <data>
    AQMDAw==
    </data>
    <key>Name</key>
    <string>AppleIntelSKLGraphicsFramebuffer</string>
    <key>Replace</key>
    <data>
    AQMEAw==
    </data>
</dict>
```

* `Rt Variables > BooterConfig > 0x28`
* `Rt Variables > CsrActiveConfig > 0x3`
* `System Parameters > Inject Kexts > Yes`
* `System Parameters > Inject System ID`

**Clover Configuration**

* Install for UEFI booting only
* Install Clover in the ESP
* Drivers64UEFI > OsxAptioFix2Drv-64

**Clover kexts**

The following kexts have been installed in `/Library/Extensions`:

* IntelMausiEthernet.kext
* FakeSMC*.kext
* FakeSMC*.kext
* VoodooHDA.kext
* USBInjectAll.kext

#### Additional tweaks ####
[up up up](#)

##### HFSPlus for VBoxHfs-64 #####
[up up up](#)

In the EFI partition `EFI/CLOVER/drivers64UEFI/` erase `VBoxHfs-64.efi` and use `HFSPlus.efi`. HFSPlus can be found in the `Install Drivers` section of [clover configurator](http://mackie100projects.altervista.org/download-clover-configurator/)

##### USB clover patch #####
[up up up](#)

In clover the `KextsToPatch` for `Comment change 15 port limit to 26 in XHCI kext` changes the port limit. The original post for the skylake Q170 HP is [this post](https://www.tonymacx86.com/threads/usb-new-raise-port-limit-patch-for-macos-10-12-sierra.202329/):

```xml
      <dict>
          <key>Comment</key>
          <string>change 15 port limit to 26 in XHCI kext (100-series) 10.12</string>
          <key>Find</key>
          <data>g710////EA==</data>
          <key>Name</key>
          <string>AppleUSBXHCIPCI</string>
          <key>Replace</key>
          <data>g710////Gw==</data>
      </dict>
```


##### SSDT gen #####
[up up up](#)

For speedstepping I can use either the `ACPI > Generate PStates` and `ACPI > Generate CStates` from clover or go with the Pike's script [ssdtPRGen.sh](https://github.com/Piker-Alpha/ssdtPRGen.sh)

I tried both and saw no performance difference and compared the graph from the Intel power gadget. So I sticked with the clover way.

the output from Pike's script:

```console
$ ./ssdtPRGen.sh
#ssdtPRGen.sh v0.9  Copyright (c) 2011-2012 by † RevoGirl
#             v6.6  Copyright (c) 2013 by † Jeroen
#             v21.4 Copyright (c) 2013-2016 by Pike R. Alpha
#-----------------------------------------------------------
#Bugs > https://github.com/Piker-Alpha/ssdtPRGen.sh/issues <
#
#System information: Mac OS X 10.12.2 (16C68)
#Brandstring: "Intel(R) Core(TM) i7-6700 CPU @ 3.40GHz"
#
#Version: models.cfg v160 / Skylake.cfg v193
#
#Scope (_PR_) {222 bytes} with ACPI Processor declarations found in DSDT (ACPI 1.0 compliant)
#Generating ssdt.dsl for a iMac17,1 with board-id Mac-B809C3757DA9BB8D
#Skylake Core i7-6700 processor [0x506E3] setup [0x0705]
#With a maximum TDP of 65 Watt, as specified by Intel
#Number logical CPUs: 8 (Core Frequency: 3400 MHz)
#Number of Turbo States: 6 (3500-4000 MHz)
#Number of P-States: 33 (800-4000 MHz)
#Injected C-States for CPU0 (C1,C3,C6,C7,C8,C9,C10)
#Injected C-States for CPU1 (C1,C2,C3,C6,C7)
#Warning: cpu-type may be set improperly (0x0705 instead of 0x0905)
#   - Clover users should read https://clover-wiki.zetam.org/Configuration/CPU#cpu_type
#Compiling: ssdt_pr.dsl
#Intel ACPI Component Architecture
#ASL Optimizing Compiler version 20140926-64 [Nov  6 2014]
#Copyright (c) 2000 - 2014 Intel Corporation
#
#ASL Input:     /Users/psebos/Library/ssdtPRGen/ssdt.dsl - 362 lines, 11192 bytes, 73 keywords
#AML Output:    /Users/psebos/Library/ssdtPRGen/ssdt.aml - 2327 bytes, 28 named objects, 45 executable opcodes
#
#Compilation complete. 0 Errors, 0 Warnings, 0 Remarks, 0 Optimizations
#Do you want to open ssdt.dsl (y/n)? n
```

* `cp ~/Library/ssdtPRGen/ssdt.aml /Volumes/EFI/EFI/CLOVER/ACPI/patched`
* `Acpi->SortedOrder add SSDT.aml to the list`

##### VoodooHDA glitch #####
[up up up](#)

While booting there is a sound loop glitch. This starts in the very first beginning when booting up (clover -v) and stops at some point on login screen. The solution is to use the latest VoodooHDA (>=2.8.9) driver which can be downloaded from [VoodooHDA site](https://sourceforge.net/projects/voodoohda/). I found it from [this post](http://www.insanelymac.com/forum/topic/314406-voodoohda-289/)

##### Video artifacts #####
[up up up](#)

**In order to make things work you MUST increase the video RAM on the BIOS.**

Apparently there are a lot of issues with the Skylake HD 530 GPU. After increasing the DVMT preallocated memory to maximum on the BIOS in order to make it boot, and patching using Multibeast there are glitches that appear on the upper left. The solution is described in [this thread](http://www.insanelymac.com/forum/topic/317551-glitch-fix-for-skylake-hd-graphicsupdate/) and in [this](https://www.tonymacx86.com/threads/skylake-intel-hd-530-graphics-glitch-fix.206410/). Prefer the insanelymac.

You need to do in addition to the inject Intel and ig-platform-id in clover:

* `Kernel And Kext Patches > KextsToPatch > change GFX0 to IGPU`. After installing it I checked with `MaciASL` that the GFX0 was actually changed in IGPU in the DSDT. I am not 100% sure that this is required

```xml
    <dict>
            <key>Comment</key>
            <string>change GFX0 to IGPU</string>
            <key>Disabled</key>
            <true/>
            <key>Find</key>
            <data>
            R0ZYMA==
            </data>
            <key>Replace</key>
            <data>
            SUdQVQ==
            </data>
    </dict>
```

* `Devices > Add Properties`. I left only the following which was the one that was required. This is based on [this insanelymac post](http://www.insanelymac.com/forum/topic/317551-glitch-fix-for-skylake-hd-graphicsupdate/) and [this tonymacx86 post](https://www.tonymacx86.com/threads/skylake-intel-hd-530-graphics-glitch-fix.206410/) that references the [PikerAlpha's post](https://pikeralpha.wordpress.com/2016/10/30/aapl-properties-for-skylake-graphics/)

```xml
    <key>AddProperties</key>
    <array>
            <dict>
                    <key>Device</key>
                    <string>IntelGFX</string>
                    <key>Key</key>
                    <string>AAPL,GfxYTile</string>
                    <key>Value</key>
                    <data>
                    AQAAAA==
                    </data>
            </dict>
    </array>
```

##### Video second monitor #####
[up up up](#)

For the Skylake HD530 there is a known issue that you cannot boot with two monitors attached. The machine during boot just panics and reboots.

I end up using aaaaa


In order to fix it you need to follow [this thread](https://www.tonymacx86.com/threads/black-screen-with-macpro-6-1-or-imac-15-or-imac-17-system-definition.183113/). Read further to see how to install it.

**HOWEVER** you cannot boot from off state with both monitors on. The procedure you have to follow will be:

1. Plug a monitor the `left/top DP port`
2. push the power switch
3. let the machine boot to the login prompt
4. login 
5. Plug in the second monitor on the `right/bottom DP port`
6. you can reboot without going through through steps 1..5
7. when you shutdown you need to go through the same procedure

There are two ways you can fix it:

**Option 1 (this is the one I chose)**

Assuming that you have a single discreet card (just the HD530) then you can apply the following patch in the `Kernel and Kext Patches` as described in the thread:

```xml
     <dict>
          <key>Comment</key>
          <string>AppleGraphicsDevicePolicy (board-id) Patch (c) Pike R. Alpha</string>
          <key>Find</key>
          <data>
          Ym9hcmQtaWQ=
          </data>
          <key>Name</key>
          <string>AppleGraphicsDevicePolicy</string>
          <key>Replace</key>
          <data>
          Ym9hcmQtaXg=
          </data>
      </dict>
```

**Option 2**

Alternative you can use `AGDPfix.v1.3.zip` which patches the `/System/Library/Extensions/AppleGraphicsControl.kext/Contents/PlugIns/AppleGraphicsDevicePolicy.kext/Contents/Info.plist` 

This is what the `AGDPfix` does:

```perl
perl -pi -e 's|[<]string[>]Config1|<string>none|' /System/Library/Extensions/AppleGraphicsControl.kext/Contents/PlugIns/AppleGraphicsDevicePolicy.kext/Contents/Info.plist;perl -pi -e 's|[<]string[>]Config2|<string>none|' /System/Library/Extensions/AppleGraphicsControl.kext/Contents/PlugIns/AppleGraphicsDevicePolicy.kext/Contents/Info.plist;perl -pi -e 's|[<]true|<false|' /System/Library/Extensions/AppleGraphicsControl.kext/Contents/PlugIns/AppleGraphicsDevicePolicy.kext/Contents/Info.plist
```

The whole script is:

```applescript
(*
Created by ShilohH on June 8th 2015.
Updated to v1.1 on Jan 6th, 2016. Added fix for iMac15/17n definitions.
Updated to v1.2 on Jan 18th, 2016. Added cache rebuild and changes backup folder naming.
Updated to v1.3 on July 11th, 2016. Added SIP Check.
*)

on run
  display dialog "The MacPro6,1 and iMac15/17 perform special functions on their specific OEM GPUs and Displays. For non-Apple hardware using the MacPro6,1 or iMac15/17 system definitions, this can cause your GPU to stop sending a signal to your monitor when OS X finishes loading.

AGDPfix will back up the AppleGraphicsControl.kex to your Desktop. It will then patch the /System/Library/Extensions/AppleGraphicsControl.kext/Contents/PlugIns/AppleGraphicsDevicePolicy.kext/Contents/Info.plist and change \"Config1\" to \"none\" for the MacPro6,1 board ID and \"Config2\" to \"none\" for the iMac15/17 board IDs. This ensures that your GPU will not be effected by functions meant for OEM Mac GPUs or Displays.

Warning:
1) You must disable System Integrity Protection (SIP) before using this app in 10.11+.
2) The AppleGraphicsDevicePolicy.kext is usually replaced or updated by OS X updates. You will usually need to use the boot argument: nv_disable=1 to disable GPU acceleration when restarting after a OS X update and then run this app again. 

AGDPfix created by shilohh. Credit to PikeRAlpha for his plist edit.

Patch Now?"
  set sip to ""
  try
    set sip to do shell script "csrutil status"
  end try
  if "enabled" is in sip then
    display dialog "SIP is enabled. Disable SIP and try again."
  else
    set patched to (do shell script "cat /System/Library/Extensions/AppleGraphicsControl.kext/Contents/PlugIns/AppleGraphicsDevicePolicy.kext/Contents/Info.plist")
    if "<string>Config1</string>" is in patched then
      set os to do shell script "sw_vers -productVersion"
      set dat to "_" & month of (current date) & "-" & day of (current date) & "-" & year of (current date) & "-" & (time of (current date))
      do shell script "mkdir ~/Desktop/AGCKextBackUp_" & os & "" & dat & " " with administrator privileges
      do shell script "cp -rf /System/Library/Extensions/AppleGraphicsControl.kext ~/Desktop/AGCKextBackUp_" & os & "" & dat & "/AppleGraphicsControl.kext" with administrator privileges
    else
      display dialog "The kext appears to be patched already. Not backing up. 
    
Run the patch again anyways?" with icon 1
    end if
    do shell script "perl -pi -e 's|[<]string[>]Config1|<string>none|' /System/Library/Extensions/AppleGraphicsControl.kext/Contents/PlugIns/AppleGraphicsDevicePolicy.kext/Contents/Info.plist;perl -pi -e 's|[<]string[>]Config2|<string>none|' /System/Library/Extensions/AppleGraphicsControl.kext/Contents/PlugIns/AppleGraphicsDevicePolicy.kext/Contents/Info.plist;perl -pi -e 's|[<]true|<false|' /System/Library/Extensions/AppleGraphicsControl.kext/Contents/PlugIns/AppleGraphicsDevicePolicy.kext/Contents/Info.plist" with administrator privileges
    do shell script "touch /System/Library/Extensions && kextcache -u /;kextcache -system-prelinked-kernel;kextcache -system-caches" with administrator privileges
    display dialog "Patching Done!
  
Restart Now?" with icon 1
    tell application "Finder"
      restart
    end tell
  end if
end run
```

This guy explains it in [this thread](https://www.tonymacx86.com/threads/skylake-intel-hd-530-integrated-graphics-working-as-of-10-11-4.188891/page-50):

> **wildwillow**: Being able to switch on or hot plug a monitor for dual displays is down to either the TV or Monitor attached, if the system see's the monitor connected at POST, its guaranteed to crash at the desktop. If the display is switched off and not recognised by the system it can then be switched on after POST. Older BIOSes automatically switched to a single monitor on boot where 100 series boots both and OS X/macOS crashes at the desktop. This is my theory and my tests have shown this to be true, hence why some need to hot plug and others switch the monitor on after POST.

In the same page **wildwillow** wrote:

> **wildwillow**: If you have iMac15,1 iMac17,1 or MacPro6,1 system definitions you need to use it to disable AppleGraphicsDeviceControl.kext. For now it will be easier for you to have it disabled.

> **wildwillow**: You have to unplug the 2nd monitor before you restart each time, this is how it works for an unknown reason that hasn't been worked out yet as far as I'm aware.
Can you boot each monitor individually and make a copy of IOReg and attach it here.
Happy New Year too!

##### Video second monitor >= 10.12.3 #####
[up up up](#)

After upgrading to 10.12.3 I have the following problem. When I lock the screen which makes the monitors sleep (and power off) my computer reboots! This is happening when I have the two monitors attached. When I just have one of them the computer behaves as expected.

I end up doing the following (**This is only required when you have two monitors attached to the computer. In case you have only one there is no problem**):

- use **Option 1** from the previous paragraph
- use the DSDT patch from [this thread](http://www.insanelymac.com/forum/topic/319802-skylake-sierra-restarts-instead-of-shut-down/) which is located here [DSDT.sakoula.zip](https://raw.github.com/sakoula/hackintosh.hp.800.g2/master/assets/DSDT.sakoula.zip)

In order to boot/reboot etc has to be the following:

- from the power off state you need to boot with a single monitor attached.
- reboot from OSX at least once with one monitor attached.
- one you have rebooted from osx and you have logged in your machine you can plug the second monitor.
- locking the computer will not reboot it.
- if you shutdown the computer you need to unplug the power for 30sec to 1min in order to be able to power it up again.
- when you need to reboot the computer always do it with one monitor plugged in.

##### Video Performance #####
[up up up](#)

When I boot I always get the following messages prior getting to the GUI:

```console
IG: HE PCI ACPI device not found - PAVP services will be disabled - add IMEI to EFI / ACPI device list
IG: Called when FB is in a non-wake State in getAttribute - attribute: 1734298985
IG: Called when FB is in a non-wake State in getAttribute - attribute: 1734298985
IG: Called when FB is in a non-wake State in getAttribute - attribute: 1734298985
IG: DRMStatus - iTunes/Apple Store Content Access Problem. Content playback may be disabled on this computer. You can continue to use the machine. but you should contact Apple.
[IGPU] Will fallback to host-side scheduling if graphics firmware fails to load
[IGPU] Chose to use graphics firmware based on platform
[IGPU] ************************************************
[IGPU] Failed to initialize graphics firmware. Failing back to host-side scheduling
[IGPU] Scheduler interface revision = 1: Default EL Scheduler
[IGPU] ************************************************
[IGPU] Graphics accelerator is using scheduler interface revision 1: Default EL Scheduler
[IGPU] Scheduler: Multiple channel indexes per command streamer
[IGPU] Scheduler: Process CSB using HWS.
[IGPU] Scheduler: PM notify enabled
[IGPU] Graphics Address: PPGTT, Separate Address Space
[IGPU] MultiForceWake Enabled: Using 3D Driver
[IGPU] Scheduler Throttle Cap = 100ms
```

on about it shows as:

```console
Intel HD Graphics 530:
Chipset Model: Intel HD Graphics 530
Type: GPU
Bus: Built-In
VRAM (Dynamic, Max): 1536 MB
Vendor: Intel (0x8086)
Device ID: 0x1912
Revision ID: 0x0006
Metal: Supported
```

However [Cinebench](https://www.maxon.net/en/products/cinebench/) gives me on 10.12.2 approx 26fps. The same computer under windows 10 performs much better giving approx 60fps which is half the performance. Similar performance applies for [LuxRender](http://www.luxmark.info/)

* Geekbench Windows GPU = 21500
* Geekbench OSX GPU = 19300
 
* CineBench Windows GPU = 55
* CineBench OSX GPU = 28
 
* LuxMark Windows GPU = 2550
* LuxMark OSX GPU = 1900
 
So:
 
* GeekBench in OSX show 90% of the performance in Windows 
* Cinebench in OSX show 50% of the performance in Windows 
* Luxmark in OSX show 75% of the performance in Windows 


Given the message I see when booting, and the performance difference, is the HD530 working with full acceleration or not under OSX?

The IORegExplorer has the following:

```console
AppleFramebuffer@0: connector-type 10 00 00 00, port-number 0x0
AppleFramebuffer@1: connector-type 00 04 00 00, port-number 0x5 - DP left
AppleFramebuffer@2: connector-type 00 04 00 00, port-number 0x6 - 
AppleFramebuffer@3: connector-type 00 04 00 00, port-number 0x7 - DP right
```

I [posted](http://www.insanelymac.com/forum/topic/319724-skylake-hd530-question/) on insanelymac and got the following answers:

```console
Denicio
Hello sakoula, Dionusis here :)
 
Indeed it seems that there's difference in Benchmarks between a number of GPUs (including AMD & NVIDIA), and it all comes down to driver implementation.

Ramalama
2 things:
OSX openGL support is very bad.
HD530 is not well supported...
```


##### Install RC scripts on target volume #####
[up up up](#)

NVRAM is used to store parameters in the parameters in the form of `MyVar=TestValue`. This is important for iMessage to work. Not all Skylake motherboards support this (although older ones support this).

In order to test it boot into OSX and give

```console
sudo nvram MyVar=TestValue
```

reboot and test whether the variable is there with 

```console
nvram -p
```

if not you need to emulate this using clover. On the clover installer select `Drivers64UEFI > EmuVariableUefi-64`. Every time the machine reboots it will load a `nvram.plist` from the disk.

In addition to that you need to enable `Install RC scripts on target volume`. Every time the machine shutdown it will save the current state of `nvram.plist`. 

Additionally the `Install RC scripts on target volume` will do clover logging that can be found at `/Library/Logs/CloverEFI`. You have to do this only for the `HDD installation`.

Check the scripts at 

* `/etc/rc.boot.d/`
* `/etc/rc.shutdown.d/`
* `/etc/rc.clover.lib`

I found this very nice thread called [Everything you need to know about NVRAM](http://www.insanelymac.com/forum/topic/298027-guide-aio-guides-for-hackintosh/page-2#entry2029552).

##### BIOS time :gun: #####
[up up up](#)

Sometimes when I reboot it detects and complains that the clock has been resetted. I setup the BIOS clock to UTC which is GMT-2 for the winter and then use bankofgreece NTP for the settings.

**I suspect that it has to do that I set up the BIOS clock with GMT+2 and then OSX tries to setup the hardware clock again. Perhaps GMT or something. And in the next reboot it complains**.

the exact message I get is:

```console
Real Time Power Loss 005
```

##### System Preferences > Energy Saver #####
[up up up](#)

* The `System Preferences > Energy Saver` panel has changed

*before*

![sierra.energy.before](https://raw.github.com/sakoula/hackintosh.hp.800.g2/master/assets/sierra.energy.before.png)

*after*

![sierra.energy.after](https://raw.github.com/sakoula/hackintosh.hp.800.g2/master/assets/sierra.energy.after.png)

> **Note**: If you do not autogenerate the P-states in clover you will get the good old panel. This is the new look of the panel and it is what you want.

##### Reboots on Shutdown #####
[up up up](#)

* When I shutdown the machine reboots

**FIXED** check [thread](http://www.insanelymac.com/forum/topic/319802-skylake-sierra-restarts-instead-of-shut-down/), by enabling `S5 Maximum Power Savings` in the BIOS.

##### Disk Renumbering #####
[up up up](#)

* Disk renumbering on reboot sometimes the SSD is `disk0` sometimes `disk1`

I updated and use the `HFSPlus.efi` driver but it keeps on renumbering

I checked `/Library/Logs/CloverEFI/boot.log` and it is always identical.

In IORegistryExplorer it is always the same:

* `IOService:/AppleACPIPlatformExpert/PCI0@0/AppleACPIPCI/SAT0@17/AppleIntelPchSeriesAHCI/PRT0@0/IOAHCIDevice@0/AppleAHCIDiskDriver/IOAHCIBlockStorageDevice/IOBlockStorageDriver/SanDisk SD7SB3Q-256G-1006 Media`
* `IOService:/AppleACPIPlatformExpert/PCI0@0/AppleACPIPCI/SAT0@17/AppleIntelPchSeriesAHCI/PRT1@1/IOAHCIDevice@0/AppleAHCIDiskDriver/IOAHCIBlockStorageDevice/IOBlockStorageDriver/ST1000DM003-1SB102 Media`

Finally I boot the machine and took two videos.

**sandisk becomes disk0**

```console
000001.538571 AppleUSBHostResources@: AppleUSBHostResources::allocateDownstramBusCurrentGated: assuming successful wakeUnits 150 sleepUnits 0
000001.538538 AppleUSBHostResources@: AppleUSBHostResources::allocateDownstramBusCurrentGated: assuming successful wakeUnits 150 sleepUnits 0
000001.538566 AppleUSBHostResources@: AppleUSBHostResources::allocateDownstramBusCurrentGated: assuming successful wakeUnits 150 sleepUnits 0
rooting via boot-uuid from /chosen: 039F81E5-E860-34CB-91D4-BBA663049474
Waiting on IOPRoviderClass > IOResources > IOResourceMatch > boot-uuid-media
Got boot device = IOService:/AppleACPIPlatformExpert/PCI0@0/AppleACPIPCI/SAT0@17/AppleIntelPchSeriesAHCI/PRT0@0/IOAHCIDevice@0/AppleAHCIDiskDriver/IOAHCIBlockStorageDevice/IOBlockStorageDriver/SanDisk SD7SB3Q-256G-1006 Media/IOGUIDPartitionScheme/Untitled 2@2
BSD root: disk0s2, major 1, minor 4
hfs: mounted Macintosh HD on device b(1,4)
XCPM: registered
VM Swap Subsystem is ON
Darwin Bootstrapper Version 4.0.0....
boot-args= -v dart=0
HID: Legacy shim 2
** /dev/rdisk0s2 (NO WRITE)
** Root file system
   Executing fsck_hfs (version hfs-366.30.3)
bash: /etc/rc.installer_cleanup: No such file or directory
Ethernet [IntelMausi]: I219LM2 (Rev. 49), xx:xx:xx:xx:xx:xx
Warning: couldn't block sleep during cache update
Warning: proceeding w/o DiskArb
/dev/disk0s2 on / (hfs,local,journaled)
bash: /etc/rc.server: No such file or directory
```

**sandisk becomes disk1**

```console
rooting via boot-uuid from /chosen: 039F81E5-E860-34CB-91D4-BBA663049474
Waiting on IOPRoviderClass > IOResources > IOResourceMatch > boot-uuid-media
Got boot device = IOService:/AppleACPIPlatformExpert/PCI0@0/AppleACPIPCI/SAT0@17/AppleIntelPchSeriesAHCI/PRT0@0/IOAHCIDevice@0/AppleAHCIDiskDriver/IOAHCIBlockStorageDevice/IOBlockStorageDriver/SanDisk SD7SB3Q-256G-1006 Media/IOGUIDPartitionScheme/Untitled 2@2
000001.538571 AppleUSBHostResources@: AppleUSBHostResources::allocateDownstramBusCurrentGated: assuming successful wakeUnits 150 sleepUnits 0
000001.538538 AppleUSBHostResources@: AppleUSBHostResources::allocateDownstramBusCurrentGated: assuming successful wakeUnits 150 sleepUnits 0
000001.538566 AppleUSBHostResources@: AppleUSBHostResources::allocateDownstramBusCurrentGated: assuming successful wakeUnits 150 sleepUnits 0
BSD root: disk1s2, major 1, minor 5
hfs: mounted Macintosh HD on device b(1,5)
XCPM: registered
VM Swap Subsystem is ON
Darwin Bootstrapper Version 4.0.0....
boot-args= -v dart=0
HID: Legacy shim 2
** /dev/rdisk1s2 (NO WRITE)
** Root file system
   Executing fsck_hfs (version hfs-366.30.3)
bash: /etc/rc.installer_cleanup: No such file or directory
Ethernet [IntelMausi]: I219LM2 (Rev. 49), xx:xx:xx:xx:xx:xx
Warning: couldn't block sleep during cache update
Warning: proceeding w/o DiskArb
/dev/disk1s2 on / (hfs,local,journaled)
bash: /etc/rc.server: No such file or directory
```

> According to the [thread](http://www.insanelymac.com/forum/topic/319802-skylake-sierra-restarts-instead-of-shut-down/) it does not matter so it can be considered solved.

##### DSDT #####
[up up up](#)

Extract DSDT with clover installed on a FAT32 stick. Boot on the loader on the main screen hit `F4`. 

Download [MaciASL](https://sourceforge.net/projects/maciasl)

Check for the patches for [pjalm](http://pjalm.com/forums/)

**The stock DSDT is located here**: [DSDT.HP800G2.zip](https://raw.github.com/sakoula/hackintosh.hp.800.g2/master/assets/DSDT.HP800G2.zip)

check the [thread](http://www.insanelymac.com/forum/topic/319802-skylake-sierra-restarts-instead-of-shut-down/)

**The patched by MaLd0n DSDT is located here**: [DSDT.sakoula.zip](https://raw.github.com/sakoula/hackintosh.hp.800.g2/master/assets/DSDT.sakoula.zip)

**But I am not using it**

### Install 10.12 my way manually ###
[up up up](#)

#### Prepare USB stick ####
[up up up](#)

format usb stick:

* GUID Partition Teble
* Name: USB
* Format: MacOS Extended (Journaled)

use command to install sierra to the stick:

```console
sudo /Applications/Install\ macOS\ Sierra.app/Contents/Resources/createinstallmedia --volume /Volumes/USB --applicationpath /Applications/Install\ macOS\ Sierra.app --nointeraction
```

#### Install Clover USB stick ####
[up up up](#)

get clover `Clover_v2.3k_r3974.zip` from [sourceforge.net](http://sourceforge.net/projects/cloverefiboot/) 

Install clover on the USB stick 'Clover_v2.3k_r3974.zip':

* Install for UEFI booting only
* Install Clover in the ESP
* Drivers64UEFI > OsxAptioFix2Drv-64

Place the [multibeast](https://raw.github.com/sakoula/hackintosh.hp.800.g2/master/assets/10.12.kexts.tgz) versions of the following kexts in `EFI/CLOVER/kexts/10.12`:

* [USBInjectAll.kext](https://bitbucket.org/RehabMan/os-x-usb-inject-all/downloads)
* [FakeSMC.kext](https://github.com/kozlek/HWSensors/releases)
* [IntelMausiEthernet.kext](https://github.com/Mieze/IntelMausiEthernet)

Make sure to delete `/EFI/CLOVER/drivers64UEFI/VBoxHfs-64.efi` and replace it with [`/EFI/CLOVER/drivers64UEFI/HFSPlus.efi`](https://raw.github.com/sakoula/hackintosh.hp.800.g2/master/assets/HFSPlus.efi.zip)

Use the [`/EFI/CLOVER/config.plist`](https://raw.github.com/sakoula/hackintosh.hp.800.g2/master/assets/unibeast.config.plist)

and you are ready

#### Install Sierra 10.12.2 ####
[up up up](#)

unplug everything but two disks / keyboard / wired mouse.

On the installer open disk utility and format the boot disk as `GUID Partition Map`/`Macintosh HD`/`Mac OS Extended (Journaled)`

#### Configure Sierra 10.12.2 ####
[up up up](#)

get clover `Clover_v2.3k_r3974.zip` from [sourceforge.net](http://sourceforge.net/projects/cloverefiboot/) 

Install clover on the Macintosh HD 'Clover_v2.3k_r3974.zip':

* Install for UEFI booting only
* Install Clover in the ESP
* Drivers64UEFI > OsxAptioFix2Drv-64
* Drivers64UEFI > EmuVariableUefi-64
* Install RC scripts on target volume
* Install Clover Preference Pane

Place the [multibeast](https://raw.github.com/sakoula/hackintosh.hp.800.g2/master/assets/10.12.kexts.tgz) versions of the following kexts in `EFI/CLOVER/kexts/10.12`:

* [USBInjectAll.kext](https://bitbucket.org/RehabMan/os-x-usb-inject-all/downloads)
* [FakeSMC*.kext](https://github.com/kozlek/HWSensors/releases)
* [IntelMausiEthernet.kext](https://github.com/Mieze/IntelMausiEthernet)
* [VoodooHDA.kext](https://github.com/Mieze/IntelMausiEthernet) **it seems that the latest Voodoo due to permissions has to be installed in /Livrary/Extensions** **in order to check the kext using the command `kextutil VoodooHDA.kext`**

Mount the EFI partition with a tool like clover configurator.

Make sure to delete `/EFI/CLOVER/drivers64UEFI/VBoxHfs-64.efi` and replace it with [`/EFI/CLOVER/drivers64UEFI/HFSPlus.efi`](https://raw.github.com/sakoula/hackintosh.hp.800.g2/master/assets/macOS-bog-G2-i7/HFSPlus.efi.zip)

Use the [`/EFI/CLOVER/config.plist`](https://raw.github.com/sakoula/hackintosh.hp.800.g2/master/assets/macOS-bog-G2-i7/10.12.3.config.plist)

Boot directly from disk and run a brief benchmark with Geekbench:

* `GeekBench x64 4.0.3 CPU` 5072/15818
* `GeekBench x64 4.0.3 GPU/OpenCl` 19516

#### System Preferences Initial Configuration ####
[up up up](#)

* Energy Saver > Turn display off after: **never** (*I noticed that when I lock the screen the display goes odd anyway, I want it on when I am there*)
* Energy Saver > Prevent computer from sleeping automatically when the display is off **checked**
* Energy Saver > Prevent computer from sleeping automtically when the display is off **checked**
* Energy Saver > Enable Power Nap **unchecked**

### Benchmarking Sierra 10.12.2 ###
[up up up](#)

* `GeekBench x64 4.0.3 CPU` 5057/15699
* `GeekBench x64 4.0.3 CPU` 5016/15412
* `GeekBench x64 4.0.3 GPU/OpenCl` 19272
* `GeekBench x64 4.0.3 GPU/OpenCl` 19068
* `CINEBENCH R15.038_RC184115 OpenGL` 28.05fps
* `CINEBENCH R15.038_RC184115 OpenGL` 26.13fps
* `CINEBENCH R15.038_RC184115 CPU` 813cb
* `CINEBENCH R15.038_RC184115 CPU` 804cb
* `LuxMark-v3.1 OpenCL GPU` 1904
* `LuxMark-v3.1 OpenCL GPU` 1807
* `LuxMark-v3.1 OpenCL CPU` 2021
* `LuxMark-v3.1 OpenCL CPU` 1971
* `LuxMark-v3.1 Native C++` 3205
* `LuxMark-v3.1 Native C++` 3109

* `AJA IBM USB2 stick / USB2 port / FRONT` WRITE: 8MB/sec / READ: 33MB/sec
* `AJA IBM USB2 stick / USB3 port / FRONT` WRITE: 8MB/sec / READ: 33MB/sec
* `AJA IBM USB2 stick / USB3 port / BACK` WRITE: 8MB/sec / READ: 33MB/sec

* `AJA ADATA USB3 stick / USB2 port / FRONT` WRITE: 27MB/sec / READ: 80MB/sec
* `AJA ADATA USB3 stick / USB3 port / FRONT` WRITE: 21MB/sec / READ: 35MB/sec
* `AJA ADATA USB3 stick / USB3 port / BACK` WRITE: 36MB/sec / READ: 84MB/sec

* `SAMSUNG T3 USB3 SSD / USB2 port / FRONT` WRITE: 36MB/sec / READ: 38MB/sec
* `SAMSUNG T3 USB3 SSD / USB3 port / FRONT` WRITE: 381MB/sec / READ: 417MB/sec
* `SAMSUNG T3 USB3 SSD / USB3 port / BACK` WRITE: 381MB/sec / READ: 418MB/sec

>**Note**: Benchmarking has been done with both monitors on and remotely connected via VNC.

>**Note**: Although the Cinebench results show half GPU performance the Geekbench and LuxMark so slightly worse GPU performance. Cinebench `19100` vs `21450` and LuxMark `1850` vs `2540`.

As I wrote in [this thread](http://www.insanelymac.com/forum/topic/319724-skylake-hd530-question/) regarding the performance:

* GeekBench in OSX show 90% of the performance in Windows 
* Cinebench in OSX show 50% of the performance in Windows 
* Luxmark in OSX show 75% of the performance in Windows 
 
I assume that it is ok since the support of the HD530 is not as good as in Windows

### HDD/SSD performance ###
[up up up](#)

* `AJA SanDisk (no trim) SD7SB3Q-256G-1006 Revision X2180006: 356MB/sec write, 412MB/sec read`
* `AJA ST1000DM003-1SB102 Revision HPH3 Serial W9A3EBP6: 181MB/sec write, 183MB/sec read`
* `AJA WDC WD10EAVS-14M4B0 Revision 01.00A01 Serial WD-WCAV56704480: 80MB/sec write, 81MB/sec read`

after reading [this](http://kb.sandisk.com/app/answers/detail/a_id/8145/~/trim-command-support-defined) it means that I have to enable TRIM on OSX

In order to do that you need to do the [following](http://macossierra-slow.com/enable-trim-third-party-ssds-macos-sierra-trimforce/):

```console
sudo trimforce enable
```

* `AJA SanDisk (with trim) SD7SB3Q-256G-1006 Revision X2180006: 339MB/sec write, 392MB/sec read`

did a check with smartctl:

**SD7SB3Q-256G-1006**

```console
smartctl -a disk0
smartctl 6.5 2016-05-07 r4318 [Darwin 16.3.0 x86_64] (local build)
Copyright (C) 2002-16, Bruce Allen, Christian Franke, www.smartmontools.org

=== START OF INFORMATION SECTION ===
Device Model:     SanDisk SD7SB3Q-256G-1006
Serial Number:    162262400672
LU WWN Device Id: 5 001b44 4a4a7ba2c
Firmware Version: X2180006
User Capacity:    256,060,514,304 bytes [256 GB]
Sector Sizes:     512 bytes logical, 4096 bytes physical
Rotation Rate:    Solid State Device
Form Factor:      2.5 inches
Device is:        Not in smartctl database [for details use: -P showall]
ATA Version is:   ACS-2 T13/2015-D revision 3
SATA Version is:  SATA 3.2, 6.0 Gb/s (current: 6.0 Gb/s)
Local Time is:    Wed Jan 18 12:33:42 2017 EET
SMART support is: Available - device has SMART capability.
SMART support is: Enabled

=== START OF READ SMART DATA SECTION ===
SMART overall-health self-assessment test result: PASSED
See vendor-specific Attribute list for marginal Attributes.

General SMART Values:
Offline data collection status:  (0x82) Offline data collection activity
          was completed without error.
          Auto Offline Data Collection: Enabled.
Self-test execution status:      (   0) The previous self-test routine completed
          without error or no self-test has ever
          been run.
Total time to complete Offline
data collection:    (  120) seconds.
Offline data collection
capabilities:        (0x5b) SMART execute Offline immediate.
          Auto Offline data collection on/off support.
          Suspend Offline collection upon new
          command.
          Offline surface scan supported.
          Self-test supported.
          No Conveyance Self-test supported.
          Selective Self-test supported.
SMART capabilities:            (0x0003) Saves SMART data before entering
          power-saving mode.
          Supports SMART auto save timer.
Error logging capability:        (0x01) Error logging supported.
          General Purpose Logging supported.
Short self-test routine
recommended polling time:    (   2) minutes.
Extended self-test routine
recommended polling time:    (  17) minutes.
SCT capabilities:          (0x003d) SCT Status supported.
          SCT Error Recovery Control supported.
          SCT Feature Control supported.
          SCT Data Table supported.

SMART Attributes Data Structure revision number: 16
Vendor Specific SMART Attributes with Thresholds:
ID# ATTRIBUTE_NAME          FLAG     VALUE WORST THRESH TYPE      UPDATED  WHEN_FAILED RAW_VALUE
  1 Raw_Read_Error_Rate     0x002f   252   252   002    Pre-fail  Always       -       0
  5 Reallocated_Sector_Ct   0x0033   252   252   005    Pre-fail  Always       -       0
  9 Power_On_Hours          0x0032   252   252   ---    Old_age   Always       -       105
 12 Power_Cycle_Count       0x0032   252   252   ---    Old_age   Always       -       194
170 Unknown_Attribute       0x0033   100   100   010    Pre-fail  Always       -       0
171 Unknown_Attribute       0x0022   100   100   ---    Old_age   Always       -       0
172 Unknown_Attribute       0x0032   100   100   ---    Old_age   Always       -       0
173 Unknown_Attribute       0x0033   100   100   005    Pre-fail  Always       -       8592031745
174 Unknown_Attribute       0x0032   252   252   ---    Old_age   Always       -       134
183 Runtime_Bad_Block       0x0032   100   100   000    Old_age   Always       -       0
184 End-to-End_Error        0x0033   100   100   097    Pre-fail  Always       -       0
187 Reported_Uncorrect      0x0032   100   100   ---    Old_age   Always       -       0
188 Command_Timeout         0x0032   100   098   ---    Old_age   Always       -       5
190 Airflow_Temperature_Cel 0x0022   072   026   045    Old_age   Always   In_the_past 28
196 Reallocated_Event_Count 0x0032   252   252   ---    Old_age   Always       -       0
199 UDMA_CRC_Error_Count    0x0022   100   100   ---    Old_age   Always       -       0
244 Unknown_Attribute       0x0032   000   100   ---    Old_age   Always       -       0

SMART Error Log Version: 1
No Errors Logged

SMART Self-test log structure revision number 1
Num  Test_Description    Status                  Remaining  LifeTime(hours)  LBA_of_first_error
# 1  Short offline       Completed without error       00%         1         -

SMART Selective self-test log data structure revision number 1
 SPAN  MIN_LBA  MAX_LBA  CURRENT_TEST_STATUS
    1        0        0  Not_testing
    2        0        0  Not_testing
    3        0        0  Not_testing
    4        0        0  Not_testing
    5        0        0  Not_testing
Selective self-test flags (0x0):
  After scanning selected spans, do NOT read-scan remainder of disk.
If Selective self-test is pending on power-up, resume after 0 minute delay.
```

**ST1000DM003-1SB102**

```console
smartctl -a disk1
smartctl 6.5 2016-05-07 r4318 [Darwin 16.3.0 x86_64] (local build)
Copyright (C) 2002-16, Bruce Allen, Christian Franke, www.smartmontools.org

=== START OF INFORMATION SECTION ===
Model Family:     Seagate Barracuda 7200.14 (AF)
Device Model:     ST1000DM003-1SB102
Serial Number:    W9A3EBP6
LU WWN Device Id: 5 000c50 09c21e777
Firmware Version: HPH3
User Capacity:    1,000,204,886,016 bytes [1.00 TB]
Sector Sizes:     512 bytes logical, 4096 bytes physical
Rotation Rate:    7200 rpm
Form Factor:      3.5 inches
Device is:        In smartctl database [for details use: -P show]
ATA Version is:   ACS-3 T13/2161-D revision 3b
SATA Version is:  SATA 3.1, 6.0 Gb/s (current: 6.0 Gb/s)
Local Time is:    Wed Jan 18 12:36:52 2017 EET
SMART support is: Available - device has SMART capability.
SMART support is: Enabled

=== START OF READ SMART DATA SECTION ===
SMART overall-health self-assessment test result: PASSED

General SMART Values:
Offline data collection status:  (0x00) Offline data collection activity
          was never started.
          Auto Offline Data Collection: Disabled.
Self-test execution status:      (   0) The previous self-test routine completed
          without error or no self-test has ever
          been run.
Total time to complete Offline
data collection:    (    0) seconds.
Offline data collection
capabilities:        (0x53) SMART execute Offline immediate.
          Auto Offline data collection on/off support.
          Suspend Offline collection upon new
          command.
          No Offline surface scan supported.
          Self-test supported.
          No Conveyance Self-test supported.
          Selective Self-test supported.
SMART capabilities:            (0x0003) Saves SMART data before entering
          power-saving mode.
          Supports SMART auto save timer.
Error logging capability:        (0x01) Error logging supported.
          General Purpose Logging supported.
Short self-test routine
recommended polling time:    (   2) minutes.
Extended self-test routine
recommended polling time:    ( 104) minutes.
SCT capabilities:          (0x10bd) SCT Status supported.
          SCT Error Recovery Control supported.
          SCT Feature Control supported.
          SCT Data Table supported.

SMART Attributes Data Structure revision number: 32
Vendor Specific SMART Attributes with Thresholds:
ID# ATTRIBUTE_NAME          FLAG     VALUE WORST THRESH TYPE      UPDATED  WHEN_FAILED RAW_VALUE
  1 Raw_Read_Error_Rate     0x002f   072   070   006    Pre-fail  Always       -       16454193
  3 Spin_Up_Time            0x0023   097   097   000    Pre-fail  Always       -       0
  4 Start_Stop_Count        0x0032   100   100   000    Old_age   Always       -       208
  5 Reallocated_Sector_Ct   0x0033   100   100   010    Pre-fail  Always       -       0
  7 Seek_Error_Rate         0x002f   060   060   030    Pre-fail  Always       -       1166329
  9 Power_On_Hours          0x0032   100   100   000    Old_age   Always       -       340
 10 Spin_Retry_Count        0x0033   100   100   097    Pre-fail  Always       -       0
 12 Power_Cycle_Count       0x0032   100   100   000    Old_age   Always       -       181
180 Unknown_HDD_Attribute   0x002a   100   100   000    Old_age   Always       -       1171349263
183 Runtime_Bad_Block       0x0032   100   100   000    Old_age   Always       -       0
184 End-to-End_Error        0x0033   100   100   097    Pre-fail  Always       -       0
187 Reported_Uncorrect      0x0032   100   100   000    Old_age   Always       -       0
188 Command_Timeout         0x0032   100   100   000    Old_age   Always       -       0 0 0
189 High_Fly_Writes         0x003a   100   100   000    Old_age   Always       -       0
190 Airflow_Temperature_Cel 0x0022   068   065   040    Old_age   Always       -       32 (Min/Max 29/32)
193 Load_Cycle_Count        0x0032   100   100   000    Old_age   Always       -       221
194 Temperature_Celsius     0x0022   032   015   000    Old_age   Always       -       32 (0 15 0 0 0)
195 Hardware_ECC_Recovered  0x003a   003   001   000    Old_age   Always       -       16454193
196 Reallocated_Event_Count 0x0032   100   100   000    Old_age   Always       -       0
197 Current_Pending_Sector  0x0032   100   100   000    Old_age   Always       -       0
198 Offline_Uncorrectable   0x0030   100   100   000    Old_age   Offline      -       0
199 UDMA_CRC_Error_Count    0x0032   200   200   000    Old_age   Always       -       0

SMART Error Log Version: 1
No Errors Logged

SMART Self-test log structure revision number 1
Num  Test_Description    Status                  Remaining  LifeTime(hours)  LBA_of_first_error
# 1  Short offline       Completed without error       00%         1         -

SMART Selective self-test log data structure revision number 1
 SPAN  MIN_LBA  MAX_LBA  CURRENT_TEST_STATUS
    1        0        0  Not_testing
    2        0        0  Not_testing
    3        0        0  Not_testing
    4        0        0  Not_testing
    5        0        0  Not_testing
Selective self-test flags (0x0):
  After scanning selected spans, do NOT read-scan remainder of disk.
If Selective self-test is pending on power-up, resume after 0 minute delay.
```

**WD10EAVS-14M4B0**

```console
smartctl -a disk2
smartctl 6.5 2016-05-07 r4318 [Darwin 16.3.0 x86_64] (local build)
Copyright (C) 2002-16, Bruce Allen, Christian Franke, www.smartmontools.org

=== START OF INFORMATION SECTION ===
Model Family:     Western Digital Caviar Green
Device Model:     WDC WD10EAVS-14M4B0
Serial Number:    WD-WCAV56704480
LU WWN Device Id: 5 0014ee 203e4fbe5
Firmware Version: 01.00A01
User Capacity:    1,000,204,886,016 bytes [1.00 TB]
Sector Size:      512 bytes logical/physical
Rotation Rate:    5400 rpm
Device is:        In smartctl database [for details use: -P show]
ATA Version is:   ATA8-ACS (minor revision not indicated)
SATA Version is:  SATA 2.6, 3.0 Gb/s
Local Time is:    Wed Jan 18 12:37:42 2017 EET
SMART support is: Available - device has SMART capability.
SMART support is: Enabled

=== START OF READ SMART DATA SECTION ===
SMART overall-health self-assessment test result: PASSED

General SMART Values:
Offline data collection status:  (0x00) Offline data collection activity
          was never started.
          Auto Offline Data Collection: Disabled.
Self-test execution status:      (   0) The previous self-test routine completed
          without error or no self-test has ever
          been run.
Total time to complete Offline
data collection:    (20760) seconds.
Offline data collection
capabilities:        (0x7b) SMART execute Offline immediate.
          Auto Offline data collection on/off support.
          Suspend Offline collection upon new
          command.
          Offline surface scan supported.
          Self-test supported.
          Conveyance Self-test supported.
          Selective Self-test supported.
SMART capabilities:            (0x0003) Saves SMART data before entering
          power-saving mode.
          Supports SMART auto save timer.
Error logging capability:        (0x01) Error logging supported.
          General Purpose Logging supported.
Short self-test routine
recommended polling time:    (   2) minutes.
Extended self-test routine
recommended polling time:    ( 239) minutes.
Conveyance self-test routine
recommended polling time:    (   5) minutes.
SCT capabilities:          (0x303f) SCT Status supported.
          SCT Error Recovery Control supported.
          SCT Feature Control supported.
          SCT Data Table supported.

SMART Attributes Data Structure revision number: 16
Vendor Specific SMART Attributes with Thresholds:
ID# ATTRIBUTE_NAME          FLAG     VALUE WORST THRESH TYPE      UPDATED  WHEN_FAILED RAW_VALUE
  1 Raw_Read_Error_Rate     0x002f   200   200   051    Pre-fail  Always       -       0
  3 Spin_Up_Time            0x0027   122   107   021    Pre-fail  Always       -       6875
  4 Start_Stop_Count        0x0032   089   089   000    Old_age   Always       -       11154
  5 Reallocated_Sector_Ct   0x0033   200   200   140    Pre-fail  Always       -       0
  7 Seek_Error_Rate         0x002e   200   200   000    Old_age   Always       -       0
  9 Power_On_Hours          0x0032   079   079   000    Old_age   Always       -       15988
 10 Spin_Retry_Count        0x0032   100   100   000    Old_age   Always       -       0
 11 Calibration_Retry_Count 0x0032   100   100   000    Old_age   Always       -       0
 12 Power_Cycle_Count       0x0032   095   095   000    Old_age   Always       -       5118
192 Power-Off_Retract_Count 0x0032   199   199   000    Old_age   Always       -       808
193 Load_Cycle_Count        0x0032   197   197   000    Old_age   Always       -       10346
194 Temperature_Celsius     0x0022   120   096   000    Old_age   Always       -       27
196 Reallocated_Event_Count 0x0032   200   200   000    Old_age   Always       -       0
197 Current_Pending_Sector  0x0032   200   200   000    Old_age   Always       -       0
198 Offline_Uncorrectable   0x0030   100   253   000    Old_age   Offline      -       0
199 UDMA_CRC_Error_Count    0x0032   200   200   000    Old_age   Always       -       798
200 Multi_Zone_Error_Rate   0x0008   100   253   000    Old_age   Offline      -       0

SMART Error Log Version: 1
No Errors Logged

SMART Self-test log structure revision number 1
No self-tests have been logged.  [To run self-tests, use: smartctl -t]

SMART Selective self-test log data structure revision number 1
 SPAN  MIN_LBA  MAX_LBA  CURRENT_TEST_STATUS
    1        0        0  Not_testing
    2        0        0  Not_testing
    3        0        0  Not_testing
    4        0        0  Not_testing
    5        0        0  Not_testing
Selective self-test flags (0x0):
  After scanning selected spans, do NOT read-scan remainder of disk.
If Selective self-test is pending on power-up, resume after 0 minute delay.
```

### Upgrade to 10.12.3 ###
[up up up](#)

check link at [tonymac](https://www.tonymacx86.com/threads/macos-10-12-3-update.212177/)

update from within the OS.

Goto `System Preferences > Clover > Check Now > Update` and follow the [Install Clover USB stick]. Your clover configuration is **not** overwritten. You have the 3974 and upgrade to 3998 and I upgrade to 4003

> **Note:** I had problems with the second monitor and making it work. It seems that I had to remove the config.plist `Kernel and Kext Patchs > AppleGraphicsDevicePolicy` patch and patch the `AppleGraphicsControl.kext`. I have not tested rebuilding all the kexts.

### References ###
[up up up](#)

* [10.11.4+ Skylake Starter Guide](http://www.tonymacx86.com/threads/10-11-4-skylake-starter-guide.193628/)
* [10.11.0-10.11.3 Skylake Starter Guide](http://www.tonymacx86.com/threads/10-11-0-10-11-3-skylake-starter-guide.179221/)
* [Skylake Intel HD 530 Integrated Graphics Working as of 10.11.4](http://www.tonymacx86.com/threads/skylake-intel-hd-530-integrated-graphics-working-as-of-10-11-4.188891/)
* [http://h20564.www2.hp.com/hpsc/swd/public/readIndex?sp4ts.oid=7633299&swLangOid=8&swEnvOid=4192](http://h20564.www2.hp.com/hpsc/swd/public/readIndex?sp4ts.oid=7633299&swLangOid=8&swEnvOid=4192)

### Software ###
[up up up](#)

* [clover configurator](http://mackie100projects.altervista.org/download-clover-configurator/)
* [ssdtPRGen.sh](https://github.com/Piker-Alpha/ssdtPRGen.sh)
* [clover](https://sourceforge.net/projects/cloverefiboot/)
* [multibeast 9.0.1](https://www.tonymacx86.com/resources/multibeast-sierra-9-0-1.329/)
* [unibeast 7.0.1](https://www.tonymacx86.com/resources/unibeast-7-0-1.320/)
* [VoodooHDA](https://sourceforge.net/projects/voodoohda/)
* [Cinebench](https://www.maxon.net/en/products/cinebench/)
* [LuxRender](http://www.luxmark.info/)
* [IORegistryExplorer_v2.1](https://www.google.gr/* search?q=IORegistryExplorer_v2.1&oq=IORegistryExplorer_v2.1&aqs=chrome..69i57j0l3.2471j0j9&sourceid=chrome&ie=UTF-8)
* [GeekBench](http://geekbench.com/)
* [Intel Power Gadget](https://software.intel.com/en-us/articles/intel-power-gadget-20)
* [MaciASL](https://sourceforge.net/projects/maciasl)
* [pjalm](http://pjalm.com/forums/)
* AJA System Test Lite @ AppStore

