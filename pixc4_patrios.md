# Instructions for bringing up the PixC4 board for Patrios Config

These instructions were developed on linux (Ubuntu) and that is highly recommended as the build environment.

Ensure environment is setup, this should be a one time thing
```
git clone https://github.com/climr/ardupilot.git
cd ardupilot
git submodule init
git submodule update
./Tools/environment_install/install-prereqs-ubuntu.sh  (this will ask for password)
git checkout Patrios-1.1   
git submodule update
python ./Tools/scripts/build_iofirmware.py
./waf configure --board PIXC4R1 --bootloader   (assuming you are using a R1 board)
./waf bootloader
./waf configure --board PIXC4R1
./waf rover  (ensure this completes without errors before proceeding)
```

If the build above is succesful, then you may move on.  START HERE IF THE ENVIRONMENT IS ALREADY SETUP

1. Install microSD card in FMU slot on the PIXC4
2. Load the iomcu bootloader
Power the board using a wall wart or power supply via the red power connector (J19 I think). Note the Zubax probe does not power the board!
connect the zubax probe to your pc and the cable to the IO MCU programming port (see silkscreen)
grab the iomcu_bl.elf file from [firmware.ardupilot.org](https://firmware.ardupilot.org/Tools/Bootloaders/)
```
cd ~
mkdir iofirmware
cd iofirmware
wget https://firmware.ardupilot.org/Tools/Bootloaders/iomcu_bl.elf
arm-none-eabi-gdb iomcu_bl.elf
tar ext /dev/ttyACM0
mon swdp_scan
attach 1
mon option erase
mon erase flash
load
kill
```
3. Load the FMU bootloader. 
```
cd ~/ardupilot
./waf configure --board PIXC4R1 --bootloader
./waf clean
./waf bootloader
```
you will end up with a file in /build/PIXC4R1/bin/AP_Bootloader.bin file
convert this to an .elf file using
`arm-none-eabi-objcopy -I binary -O elf32-little --change-section-address .data=0x08000000 AP_Bootloader.bin output.elf`

now switch your Zubax probe so that it's connected to the FMU debug port
```
arm-none-eabi-gdb output.elf
tar ext /dev/ttyACM0
mon swdp_scan
attach 1
mon option erase
mon erase flash
load
kill
```
4. Now we are ready to build the FMU firmware. We should be done with the Zubax probe at this point so you can unplug it. Also unplug the main power, from this point forward in the setup process we can use the USB power.

Connect a USB between the PC and the FMU USB port 
```
cd ~/ardupilot
./waf configure --board PIXC4R1
./waf clean
./waf rover --upload
```

This will now build the rover project (may take a while) and when it's done it will try to upload to the board. The bootloader can only accept uploads for a short period after power-on, so once the build is done and it's waiting to upload, take out and plug back in the USB cable and that should power cycle the board and the bootloader should immediately allow the firmware upload and you'll see this progressing on the screen.

Once the upload is done and verified, you can power cycle again. The FMU and IO LEDs should flash, perhaps go solid for a bit and then go off indicating that there are no errors. 

## Now you can switch to windows for the remainder of the setup. If you don't have QGroundControl installed, download it and install now
5. Connect QGroundControl to the PIC4 using USB cable
6. Set SYSID_THISMAV parameter to desired sysid for this system
7. Edit reference param file to have matching SYSID_THISMAV
Load param file
Reboot flight controller
Connect QGCS using USB, check following params
	For Patrios box, set orientation to pitch_180
      Check failsafes, make sure radio FS is disabled, GCS battery is enabled
      Reboot flight controller
Connect QGCS using USB
Do accelerometer calibration
Do compass calibration
Check for proper HUD activity in the QGCS flight display
Now the pixc4 board is ready to be installed in a Patrios box
Once installed in box
   Connect using a radio
   QGroundControl set power type CubeOrange
   Measure battery voltage and use calculate and set to enter actual voltage
   Enter 0.5A for current calculate and set
Test and checkout
Check front and rear steering servos
Check front and rear ESC output
Check gimbal output
Check front and rear light output
Check spare output
Check compass orientation
Check for GPS lock
