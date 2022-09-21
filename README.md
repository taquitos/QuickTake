# QuickTake 150 on the M1 Max #
## Hardware requirements ##
- QuickTake 150 (obvi)
- `DB9 Female to Mini Din 8-pin Male` cable ([Amazon link](https://www.amazon.com/dp/B00ALULFX0))
- `DB-9 RS-232 Converter Cable` with a `Prolific Chipset` ([Amazon link](https://www.amazon.com/dp/B00IDSM6BW))
- A `USB C to USB dongle` ([Amazon link](https://www.amazon.com/dp/B07BS8SRWH)).


## Windows XP ##
### UTM (x86 emulator) ###
1. Install [UTM via GitHub](https://github.com/utmapp/UTM/releases/latest/download/UTM.dmg).
2. Download and unzip the [XP Template for UTM](https://github.com/utmapp/vm-downloads/releases/download/windows-template/windows-xp-x64-utm.zip).
3. Grab the XP disc labeled [`en_windows_xp_professional_sp3_Nov_2013_Incl_SATA_Drivers.iso` from Archive.org](https://archive.org/download/WIndows-XP-Professional-SP3/en_windows_xp_professional_sp3_Nov_2013_Incl_SATA_Drivers.iso).
4. Follow [these install instructions](https://mac.getutm.app/gallery/windows-xp), but ignore the last instruction for installing Firefox.
5. During install, it might give you an option for naming the machine, use `quicktakexp` as the name
6. Install the most current [Opera for Windows XP](https://www.opera.com/computer/thanks?partner=www&par=id=39912%26location=415&gaprod=operawinxpvista).

### File sharing in Windows ###
The best way to get files between XP and your host system is to setup filesharing. You'll need to turn that on in your client system. The way I did it was:

1. Create a `Share` folder on your Windows XP Desktop.
2. Right-click then `Sharing` tab.
3. Enable `Share this folder on the network`.
4. Enable `Allow network users to change my files`.
  - This should configure your Windows XP firewall properly, but you can doublt check in Firewall setting that under `Exceptions` -> `Programs and Services` you see `File and Printer Sharing` is enabled.
5. Shutdown Windows.
6. In UTM open your UTM Windows XP configuration and select `network`. Ensure it's set to `Shared Network` and that the `Emulated Network Card` is `rtl8139`.
7. Select `IP Configuration` sub-menu and ensure `Isolate Guest from Host` is **not** checked.
8. Start machine, once booted you should be able to open `Finder` on you mac and using `Go` -> `Connect to Server` you put in `smb://quicktakexp`.
	- If you didn't name the pc `quicktakexp` during install, you'll need to go to `Start` -> `Settings` -> `Control Panel` -> `System` -> `Computer Name` -> click button `Change` -> Enter in `quicktakexp` and then click `OK` -> `OK` and then restart Windows.
9. Once the `Enter your name and password for the server "quicktakexp"` prompt is on-screen, switch to `Guest` and press `Connect`. 
10. Press `OK` on the next screen (should show your shared folder `Share`.

	
### QuickTake 1.5 for Windows ###
To support the QuickTake 150, you'll need the 1.5 version of the Windows QuickTake softare. You can do this on your mac and copy it via the Share you setup to the Windows XP vm.

1. Download the QuickTake 1.5 software from [this janky website](https://www.solvusoft.com/en/update/drivers/digital-camera/apple/quicktake/150/model-numbers/) - You'll want to click on the `d74152.zip (Download)` link, which will give you a pop-up. Click on `No thanks, download Apple QuickTake 150 driver`, solve the captcha and it will download a zip named `d74152.zip`.
2. Unzip `d74152.zip` into the `Share` folder.
3. In Windows, run the `SETUP.EXE` file, and accepts the defaults.

### Install Serial Port Drivers for Windows
When you first connect your USB to Serial port adaptor, you'll need to install the drivers in Windows. 

1. Plug-in your serial port adaptor.
2. In the upper-right corner of the VM window, you'll see a little icon that is the 3rd from the right that looks like the end of a plug. Click it.
3. You should see an entry for the USB Serial device. Click that.
4. Click `Always Allow` if you get a warning that you are connecting a device to the client.
5. Windows will then recognize the device, but it won't have the software for it. Just ignore that for now, don't do anything with it (don't close it).
6. Switch back to the mac-side of things and download the Windows serial port drivers from [the manufacturer's website](https://www.prolific.com.tw/UserFiles/files/PL23XX_Prolific_DriverInstaller_v408.zip).
7. Unzip and copy the `PL23XX_Prolific_DriverInstaller_v400` folder into your `Share` folder. It might be named `PL23XX_Prolific_DriverInstaller_v4<some version>` They seem to update it regularly. I use the `v400`
8. From Windows, go into the `Share` folder and then inside the `PL23XX...` folder you just moved, and find the `.exe` file that starts with `PL23XX-M_LogoDriver_Setup_<version>.exe` and launch it.
9. Accept the defaults for the installer.
10. Once installed, go back to the Windows diver install window and click `Next` so that it will just do the `Recommended` action. This should find the newly installed driver.

### Viewing and Exporting Photos ###

#### Initial setup (only once)
With the camera connected to the 3 dongle-hell chain, and plugged in to the computer.

1. Open `Programs` -> `Apple QuickTake` -> `QuickTake Serial Ports`.
2. Select `COM 3`. You should see that it is available.
3. Select `57600` for the baud.
4. Press `Test` - This should pass. If not, check that you shared the USB Serial adaptor with the client machine like you did during driver install.
5. Press `OK`
6. Open `Programs` -> `Apple QuickTake` -> `QuickTake`.
7. Navigate to `Camera` -> `Move all Camera Images to Disk..`
8. Select the `Share` folder `DOCUME~\<username>\Desktop\Share` as the destination.
9. To export as BPM, once imported, you'll need to go to `File` -> `Open...` then select a single photo.
10. Once open, navigate to `File` -> `Save As...` -> `Save File as Type:` 

### **NOTE:** ###
_**Importing into the same folder that already has images WILL OVERWRITE EXISTING IMAGES WITH THE SAME NAME WITHOUT WARNING.**_