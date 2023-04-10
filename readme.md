After about 10 years, that motherboard still runs fairly well and has many things that can be improved.

Here is a custom BIOS with many fixes :

- Enabled all advanced settings (be careful with what you do, some might damage your hardware)
- Fixed GOP support (read below for details)
- Fixed HDA for macOS compatibility
- Unlocked MSR for macOS compatibility
- Made room for bigger logo and updated with a nice one (1080p)
- Lowered minimum fan speed limit
- Management Engine updated to 8.1.70.1590
- OPROMs updated to :
Intel RST for SATA - 10.5.1.1070
Intel Boot Agent GE - 1.5.62
Marvell 88SE9120 - 1.0.0.1029
Marvell 88SE9172 - 1.0.0.0022
Marvell 88SE917A - 1.0.0.0022
Marvell 88SE91A0 - 1.0.0.1029
JMicron JMB36x - 1.08.01


About GOP support :
As far as I could figure, GOP didn’t work because of a stupid bug : the CSM is loaded even when EFI rom is selected, thus loading two drivers that conflict.
Thank ASUS, this is probably the “persistent perfection” they talk about in their boot logo 
After replacing CSMVideo with HermitCSM (thanks to Hermit Crab Labs for the driver), the CSM module still gets loaded but at least it doesn’t prevent GOP from working.
To enable GOP you just need to set “PCI ROM Priority” to “EFI compatible” in “Boot” pane.
It is highly recommended that you disable OPROMs as they seem to interfere with logo display either by making it not appear or appear corrupt.
This was tested with dual graphics with RX560 and RX580 (3 displays).
This fix can probably be applied to all other P67 family boards from ASUS that suffer the same bug and also many other boards with different chipsets but same generation (like Z68).

```diff
-     About “Above 4G decoding” :
-     You must pay special attention to the status of \ Advanced \ PCI Subsystem Settings \ Above 4G decoding, whether you use legacy or GOP display init.
-     - Above 4G decoding must be disabled when running display in legacy mode
-     - Above 4G decoding must be enabled when running display in GOP mode
-     Other combinations will result in getting video signal with black screen and no POST.
-     Those results were obtained using RX580 on PCIEX16_1 and RX560 PCIEX16_2, your mileage may vary depending on your configuration.
-     In case of problem use the reset CMOS jumper to get back to bootable state.
```

Known issues :
- Logo/POST appear on secondary PEG with multiple graphic cards but setup interface and boot selection menu appear on the primary, there’s an option to choose the primary PEG port but it doesn’t seem to work, so I’ve let it hidden.
If you mind having displays switching, as workaround, you can make everything display on secondary by enabling any opROM in “On board devices” menu and setting “Option ROM Messages” to “force BIOS” in “Boot” menu. OS boot display will also be secondary.
- Mouse does not work in advanced BIOS display mode with multiple graphic cards, but is OK in easy mode, and subtools like EZ flash. If you launch one tool and go back to main menu, mouse then works in advanced mode. Mouse sometimes work if you set display configuration as described in issue above.
- No logo during windows boot. This is because BGRT ACPI table still has display bit status set to 0. I suspect this is because something does switch display mode during POST which in turns disables this bit. However the logo is present in memory and the BGRT table is valid, a workaround to force logo display like using Opencore or configuring Windows’ boot loader can get it displayed.

About the many new options :
You’ll see there are tons of options. So far, the options I tried run fairly well and do work, but I’m far from having tested them all, if you notice something that doesn’t work, or is irrelevant for this mobo, please report.
Some irrelevant settings were left disabled (not working or intended to absent devices like integrated GPU or battery…)

Other notes :
- Due to limited space, it was not possible to have HD logo and NVME at same time. Two versions are proposed :
P8P67Pro3602HQlogo without NVME and with high quality logo
P8P67Pro3602nvme with NVME and default logo


See screenshots folder to see detail of available options (there might be little differences here and there, not all are up to date)
A HD background with better quality was added to make good use of the now working GOP driver. 
![alt text](https://github.com/Krazubu/P8P67ProHacks/blob/main/Screenshots/dd3d5144342842b66247d5a9caab323ed55c8706.jpg)


Changelog :
version 1.0 : 1st version

version 1.1 : disabled entries for absent hardware or not working

version 1.2 : disabled more useless entries, added NVME and made 2 versions to chose between HQ logo or NVME

```diff
- WARNING !!! THOSE BIOS CONTAIN NO FD44 DATA, BRING YOURS USING FD44 TOOL BEFORE FLASHING !!!
```
P8P67Pro3602HQlogo.zip (3.34 MB)

P8P67Pro3602nvme.zip (3.27 MB)
