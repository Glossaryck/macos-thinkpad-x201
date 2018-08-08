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

**_I. Preparation_**
You will need:
- A thumb drive of at least 8GB
- A computer with macOS preinstalled (a virtual machine works as well, I recommend [this](https://techsviewer.com/how-to-install-mac-os-x-el-capitan-on-vmware-on-pc/)
- An Apple ID

**_II. Creating the installation medium_**
Creating the installer manually is recommended since UniBeast may have bugs on this particular machine.
- Grab a copy of macOS Sierra from the App Store on the prepared OS X machine.
- Right click on the installer app you just downloaded and select Show Package Contents.
- Open the Contents folder.
- Open the SharedSupport folder.
- Mount the InstallESD.dmg disk image.
- Launch Terminal and execute these commands:
```
 defaults write com.apple.finder AppleShowAllFiles YES
 killall Finder
```
The hidden file on the OS X Install ESD disk image will now show up.
- Mount the BaseSystem.dmg file from the OS X Install ESD disk image.
- Fire up Disk Utility and format the target USB in HFS+ (Journaled)
- Restore the target USB from the OS X Base System drive on the left pane.
- When finished, open the newly restored USB drive.
- Navigate to System/Installation and delete the Packages alias.
- Copy the Packages folder from OS X Install ESD to the Installation folder mentioned above.
- Copy everything from the folder, excluding the Packages folder, to the root of the target USB drive.
- Install Clover on the USB.
 Use the installer from tools.zip above, change install location to the target USB drive.
 Then customize the installation as follows:
```
 + Expand the Bootloader section and check "Install boot0af in MBR"
 + Expand the CloverEFI section and check "CloverEFI 64-bits SATA"
 + Leave every section not mentioned above unchecked.
```
**_III. Modifing the installation medium_**
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

**_IV.Install macOS_**
- Boot into the USB drive. The Installer should now start up.
- Launch Disk Utility and erase your destination volume.
- Follow the instructions and install as normal.

**_V. Post-installation_**
- Boot to the installer drive.
- Launch Terminal and execute these commands:
```
 cd /Volumes/Macintosh\ HD/System/Library/Extensions
 rm -rf AppleIntelHDGraphics.kext
 rm -rf AppleIntelHDGraphicsFB.kext
```
- Boot to the partition where you installed OS X. Follow the onscreen instructions to finish the installation. 
After completing the initial setup instructions, your desktop should now appear.

**_VI. Post-installation: Installing Clover_**
Install and config Clover on your macOS volume, using the exact same step you did with your installation medium in steps II and III.

**macOS should now be up and running on your X201!**

**_Extras:_**

Disable Hibernation and related options: used to deal with hibernation related problems
```
    sudo pmset -a hibernatemode 0
    sudo rm /var/vm/sleepimage
    sudo mkdir /var/vm/sleepimage
    sudo pmset -a standby 0
    sudo pmset -a autopoweroff 0
```
