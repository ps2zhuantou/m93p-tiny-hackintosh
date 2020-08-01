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
Wi-Fi            Broadcom BCM94352HMB
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

 ## What Doesn't Work

 This build is still a bit away from being golden. Here are my current issues:

 - no fan readout
 - no sleep
 - still a bit shaky energy management (though it could be that the CPU is just busy most of the time since it clocks up and don just fine)


# Useful Links
https://dortania.github.io/OpenCore-Desktop-Guide/ - The OpenCore Desktop Guide is the most important bit of info for this build.

# Tools To Download
https://github.com/corpnewt/gibMacOS - gibMacOS lets you download MacOS recovery images directly from Apple.

https://github.com/corpnewt/ProperTree - ProperTree is a nice plist editor. It's cross platform, so you can use it both when preparing the files in Windows and later in MacOS.

https://github.com/corpnewt/GenSMBIOS - GenSMBIOS is used to create a valid serial number for your hackintosh so you can use iMessage and other iCloud services later.
# Creating the Hackintosh

## Creating the USB Installer
Strongly recommend you go through [this video](https://www.youtube.com/watch?v=M1pnWKNaqUs). It's another working example of M93p Hackintosh but different hardware spec.

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





[1]:https://dortania.github.io/OpenCore-Desktop-Guide/post-install/iservices.html
[2]: https://dortania.github.io/OpenCore-Desktop-Guide/config.plist/haswell.html#platforminfo
[3]: https://dortania.github.io/OpenCore-Desktop-Guide/installer-guide/winblows-install.html
[4]: https://www.tonymacx86.com/threads/dell-optiplex-7020-4k-monitors-on-intel-4600-integrated-gpu.282589/page-16#post-2157430