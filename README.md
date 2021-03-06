# Windows 10 Professional Annoyance-Free Automated USB Install

## Introduction
A guide to creating a Windows 10 install without all of the "extras" - disabled One Drive, Soda Crush, etc., as well as turning off annoying prompts and online account registration. 

The goal of this setup was to create a USB key that could be used to easily create a hassle-free desktop install method for a gaming PC, but may suit other purposes as well. 

## TOO LONG, WON'T READ

Fair enough, if you've already got Windows installed (or upgraded from something else, perchance?) and want to rid it of the numerous annoyances with one quick script, you can find that in the "PostInstall" folder. This script is from [4chan](http://boards.4chan.org/g/thread/49538199/windows-10-general-thread-30) and is actually very handy, but my goal is to stop this from happening in the first place, rather than fixing it after the fact.

To run it: 
* Press the Windows key
* Type "powershell"
* Right click on "powershell" and select "run as administrator"
* Paste the following: `Set-ExecutionPolicy RemoteSigned`
* Run `4chan-win10.ps1`

## Download Windows 10
Use the [Windows media creation tool](https://www.microsoft.com/en-ca/software-download/windows10) to get a copy of Windows 10. Once your media is created, git clone this repository (or donwload a zip) and add all of the files to the root of the USB stick. 

## O&O Shut Up 10
I used [O&O Shut Up 10](https://www.oo-software.com/en/shutup10) to take care of many of the annoyances here, using the `ooshutup10.cfg` file as a parameter. Download this tool, unzip it and place the `OOSU10.exe` file in the root of the USB stick as well. 

## A Head Start
I've included two sample files that can be used once you have a Windows 10 install USB stick created: `autounattend.xml` and `customize.xml`.

Note, however, that they are customized to my liking. In particular, you will want to query/replace the username `winadmin` and the password `muggles` from within `autounattend.xml`. Otherwise, I think the defaults are pretty rational. 

Should you wish to simply make your own starter file, the [Windows Answer File Generator](http://windowsafg.no-ip.org/win10x86_x64.html) is a big help. 

Furthermore, the `sysprep.bat` and `customize.xml` files assumes that you've only got one CDROM, one HDD and one USB stick attached to your Virtual Box VM. If this is not the case, chances are your WIM file will not be located in `E:/sources/install.wim#Windows 10 Pro` and the sysprep command parameter `/unattend:e:\customize.xml` should also be edited in turn. 

You can stop here if it worked for you, otherwise, continue along for testing, and modifications. 
## Download Virtual Box
We'll use Virtual Box to prepare the USB stick because it runs on [Windows](http://download.virtualbox.org/virtualbox/5.0.24/VirtualBox-5.0.24-108355-Win.exe), [Mac](http://download.virtualbox.org/virtualbox/5.0.24/VirtualBox-5.0.24-108355-OSX.dmg) and [Linux](https://www.virtualbox.org/wiki/Linux_Downloads)
## Download Plop
As we're using Virtual Box, we'll bootstrap the USB boot when testing with a tiny [Plop Boot Manager ISO](https://download.plop.at/files/bootmngr/plpbt-5.0.15.zip). Note that on some platforms you can get this done with the Virtual Box tools and accepting a further agreement. 

Once downloaed, unzip the ISO to a location you'll use in a moment when creating your virtual machine.
## Create a Test Virtual Machine
Create a Windows 10 64bit virtual machine in Virtual Box, making sure to attach you newly-created USB stick to it.
## First Boot
Continue the install as usual until you are met with the "express"/"custom install" screen. At this point press ctrl+shift+f3 in order to get into audit mode. 
## Audit Modes
You will be presented with a sysprep window - simply cancel it for now. 
## Default Profile
Make the changes you want to see in default profiles now. 

Steps to take:
* Start a command prompt by pressing win+r, then typing "cmd"
* Run O&O Shutup 10 like so: `e:/OOSU10.exe ooshutup10.cfg /silent /nosrp`
* Disable the consumer content (games, etc.): `regedit /s e:/disableconsumerfeatures.reg`
* Uninstall OneDrive: `e:/uninstall_onedrive.bat`

## Sysprep
Once you are done customizing your default user, run the sysprep command by double-clicking on `sysprep.bat` in your USB stick. 
## Unattended Configuration File
Now it's time to create your unattended config file. You can use mine, but you may be better served using the wizard here: http://windowsafg.no-ip.org/win10x86_x64.html 

Add this file to the root of your USB stick. 
## Image to WIM
## Convert to ESD
## Test Install
Finally, test the installation one last time by removing your virtual hard drive and creating a new one - this will make sure the changes made in our captured install are no longer present. No need for "ctrl+shift+f3" - just install as you would normally, minus all of the nags, prompts and apps installed post-setup. 
## Using the USB Disk
You can use this USB stick whenever you want to perofrm a new Windows 10 install!

## Resources Used 

* https://blogs.technet.microsoft.com/mniehaus/2015/11/11/removing-windows-10-in-box-apps-during-a-task-sequence/
* http://www.tenforums.com/tutorials/3020-windows-10-image-customize-audit-mode-sysprep.html
* https://msdn.microsoft.com/en-us/windows/hardware/commercialize/manufacture/desktop/boot-windows-to-audit-mode-or-oobe
* http://www.virten.net/2014/12/howto-usb-boot-a-vm-in-vmware-workstation-11/
* https://technet.microsoft.com/en-ca/library/dn621902.aspx
* https://technet.microsoft.com/en-us/itpro/windows/deploy/create-a-windows-10-reference-image
* http://windowsafg.no-ip.org/win10x86_x64.html
* https://github.com/alek-sys/sublimetext_indentxml
* http://winaero.com/blog/fix-windows-10-installs-apps-like-candy-crush-soda-saga-automatically/
* https://www.oo-software.com/en/shutup10
* https://www.winreducer.net/winreducer-es-wim-converter.html
* http://answers.microsoft.com/en-us/onedrive/forum/odoptions-oddesktop/how-to-uninstall-onedrive-completely-in-windows-10/e735a3b8-09f1-40e2-89c3-b93cf7fe6994
* https://en.wikibooks.org/wiki/Windows_Batch_Scripting#Functions
* https://ndswanson.wordpress.com/2014/10/24/inject-drivers-to-windows-10-install-media/
* https://dismgui.codeplex.com/releases/view/133331
* https://github.com/dfkt/win10-unfuck/blob/master/4chan-g-scripts-UNTESTED/Debloat-Windows10.ps1
