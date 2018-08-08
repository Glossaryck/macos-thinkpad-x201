# macos-thinkpad-x201
**(Underdeveloped) macOS Sierra on a ThinkPad X201?**

**Working:**
- Intel HD Graphics 288MB with QE/CI
- RAM DDR3 at 1334MHz
- Keyboard + Touchpad
- Ethernet (LAN)
- Bluetooth
- USB

**Not (yet) working:**
- Wi-Fi (built-in card not natively supported)
- Sound (AppleHDA not yet patched)
- Hibernation
- VGA Output (no support)

**_Preparation_**
```
You will need:
- A thumb drive of at least 8GB
- A computer with macOS preinstalled (a virtual machine works as well, I recommend [this](https://techsviewer.com/how-to-install-mac-os-x-el-capitan-on-vmware-on-pc/)
- An Apple ID
```

**_Creating the installation medium_**
```
Creating the installer manually is recommended since UniBeast may have bugs on this particular machine.
- Grab a copy of macOS Sierra from the App Store on the prepared OS X machine.
- Right click on the installer app you just downloaded and select Show Package Contents.
- Open the Contents folder.
- Open the SharedSupport folder.
- Mount the InstallESD.dmg disk image.
- Launch Terminal and execute these commands:
 defaults write com.apple.finder AppleShowAllFiles YES
 killall Finder
The hidden file on the OS X Install ESD disk image will now show up.
- Mount the BaseSystem.dmg file from the OS X Install ESD disk image.
- Fire up Disk Utility and format the target USB in HFS+ (Journaled)
- Restore the target USB from the OS X Base System drive on the left pane.
- When finished, open the newly restored USB drive.
- Navigate to System/Installation and delete the Packages alias.
- Copy the Packages folder from OS X Install ESD to the Installation folder mentioned above.
- Copy everything from the folder, excluding the Packages folder, to the root of the target USB drive.
- Install Clover on the USB.
 Use the installer from tools.zip above, change install location to the target USB drive, then customize the installation as follows:
 + Expand the Bootloader section and check "Install boot0af in MBR"
 + Expand the CloverEFI section and check "CloverEFI 64-bits SATA"
 + Leave every section not mentioned above unchecked.
```

**_Modifing the installation medium_**
```
- First off, on the installation USB go to System/Library/Extensions and delete:
 AppleIntelHDGraphics.kext
 AppleIntelHDGraphicsFB.kext
- Now go to EFI/CLOVER/drivers64/
- Remove everything from the above folder and put the files in the drivers64UEFI folder from this repository in it.
- Repeat the process with the drivers64UEFI folder in EFI/Clover/
- In EFI/CLOVER/kexts/ delete everything excluding the "Other" folder.
- Copy the kexts in the kext folder from this repo to the "Other" folder.
- Finally, replace the config.plist file in EFI/CLOVER/ with the config.plist file found in this repo
- You should now be able to boot to the installer using the USB.
```
