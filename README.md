# m93p-tiny-hackintosh
a Repository containing files for turning a Lenovo M93p tiny into a shiny hackintosh
# Introduction

This repo is based on the awesome [m93p-tiny-hackintosh](https://github.com/lumpi2k/m93p-tiny-hackintosh) repo, and fixed 4K display issue.

 ## My Configuration

```
Model            Lenovo ThinkCentre M93p Tiny
Motherboard      Intel Q87 
CPU              Intel Core i5-4570T
Memory           16G DDR3 1600MHz (8G+8G)
Graphics         Intel HD 4600
Audio Realtek    ALC283
Ethernet         Intel I217LM
Wi-Fi            Broadcom BCM94352HMB (Azurewave aw-ce123h)
Monitor          Dell U3219Q 3840x2160
BIOS             FBKTDEAUS 06/16/2020 
macOS            Catalina 10.15.6
OpenCore         0.5.8
```

 ## What Works

 - Catalina 10.15.6
 - iMessage
 - Airdrop
 - 4K60 through DP
 - Audio Out
 - Energy Management
 - FileVault
 - App Store
 - Wifi and Bluetooth
 - Sleep

 ## What Doesn't Work

 This build is still a bit away from being golden. Here are my current issues:
 - Internal speaker
 - Bluetooth sometimes does not function, it's on but not discoverable and cannot find other device as well
 - No fan readout
 - Energy management, I didnd't see much difference with different configurations

# BIOS

 - Devices -> Video Setup -> Pre-Allocated memory Size = 128MB
 - Advanced -> CPU Setup -> VT-d = Disabled
 - Security -> Secure Boot -> Secure Boot = Disabled
 - Startup: CSM = Enabled, BOot Mode = UEFI Only

# HD4600 & 4K & FrameBuffer

To enable 4K support on HD4600, besides device-id, below properties should be updated as well
- framebuffer-unifiedmem : 2048MB
- framebuffer-stolenmem : 128MB (default=32MB)
- framebuffer-fbmem : 48MB (default=19MB)

framebuffer (Original): 0300220D 00030303 00000002 00003001. In this original setting, framebuffer-stolenmem=00000002 (32MB), framebuffer-fbmem=00003001 (19MB).

Common Resolution to required framebuffer memory size, required memory = 4octets x resolution.
(4 octets because RGBA color)

| Resolution  | Required fbmem  |
|---|---|
| 1080p  | 7.9 MB  |
| 1440p  | 14 MB  |
| 2160p | 31.6 MB  |
That's why we get 1440p on a 4K monitor without any patch, and have to increase framebuffer-fbmem to 32MB for 4K.

Because framebuffer-fbmem is increased to 32MB, to make sure cbmem+cursormem < stolenmem, stolenmem should be increased as well. I tried 64mb and 128mb, both works, in current build 128mb is used.

## More Reading on FrameBuffer
- [Intel Framebuffer patching using WhateverGreen](https://www.insanelymac.com/forum/topic/334899-intel-framebuffer-patching-using-whatevergreen/?do=findComment&comment=2647254)
- [Troubleshooting 4k on HD 4600 iGPU (OpenCore), working temp patch and help requested](https://www.reddit.com/r/hackintosh/comments/h0z4yf/troubleshooting_4k_on_hd_4600_igpu_opencore/)


# WIFI & Bluetooth
work in progress

# Audio & AppleALC
work in progress

# Useful Links
https://dortania.github.io/OpenCore-Desktop-Guide/ - The OpenCore Desktop Guide is the most important bit of info for this build.

# Tools To Download
https://github.com/corpnewt/gibMacOS - gibMacOS lets you download MacOS recovery images directly from Apple.

https://github.com/corpnewt/ProperTree - ProperTree is a nice plist editor. It's cross platform, so you can use it both when preparing the files in Windows and later in MacOS.

https://github.com/corpnewt/GenSMBIOS - GenSMBIOS is used to create a valid serial number for your hackintosh so you can use iMessage and other iCloud services later.
# Creating the Hackintosh

## Creating the USB Installer
Strongly recommend you go through [this video](https://youtu.be/3zsfkVZ2Y-E). It's another working example of M93p Hackintosh but different hardware spec.

## Preparation of the EFI Folder
First you should clone or download this repo and the tools I recommended. If you have the same system config as my Lenovo tiny PC there's only one thing you have to set in the config.plist located under EFI/OC/

As you can see in https://github.com/lumpi2k/m93p-tiny-hackintosh/blob/caf13bc817f28be0f482b7b1a78febe9a7ae8a4b/EFI/OC/config.plist#L769-L798

```
	<key>PlatformInfo</key>
	<dict>
		<key>Automatic</key>
		<true/>
		<key>Generic</key>
		<dict>
			<key>AdviseWindows</key>
			<false/>
			<key>MLB</key>
			<string>PUT YOUR OWN HERE</string>
			<key>ROM</key>
			<data>AAAAAAA=</data>
			<key>SpoofVendor</key>
			<true/>
			<key>SystemProductName</key>
			<string>iMac15,1</string>
			<key>SystemSerialNumber</key>
			<string>PUT YOUR OWN HERE</string>
			<key>SystemUUID</key>
			<string>PUT YOUR OWN HERE</string>
		</dict>
		<key>UpdateDataHub</key>
		<true/>
		<key>UpdateNVRAM</key>
		<true/>
		<key>UpdateSMBIOS</key>
		<true/>
		<key>UpdateSMBIOSMode</key>
		<string>Create</string>
	</dict>
```

I blanked my own serial info in the config.plist. This means you'll have to create your very own set of serials.

The OpenCore Desktop Guide shows this in it's chapter  [Fixing iServices][1]. **Don't skip this step, it is very important!** I've left the model iMac15,1 in the config, but you can also use iMac14,1 or iMac14,2 (these should technically work a little better since they're the exact CPU generation. I found no difference though.)

After you've created the SMBIOS with GenSMBIOS put in the values as described [here in the Desktop Guide][2].

# Reference Links:
* https://dortania.github.io/OpenCore-Desktop-Guide/post-install/iservices.html
* https://dortania.github.io/OpenCore-Desktop-Guide/config.plist/haswell.html#platforminfo
* https://dortania.github.io/OpenCore-Desktop-Guide/installer-guide/winblows-install.html
* https://www.tonymacx86.com/threads/dell-optiplex-7020-4k-monitors-on-intel-4600-integrated-gpu.282589/page-16#post-2157430