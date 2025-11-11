# #savethecorvus
Preservation of Corvus Hard Disk Drives for Apple II Computers. Focusing on the Custom ProDOS/DOS 3.3 firmware for the Parallel Apple II Interface Card I created in 1984

### Disclaimer
This is provided AS-IS with NO WARRANTY. Derived from reverse engineering the firmware for the Corvus Apple II Parallel Interface Card; original Corvus IP may apply—use responsibly.
Backup all Corvus hard disk data before converting from the original Corvus firmware to the ProROM firmware.
WARNING: No information stored on the hard drive with the original Corvus firmware will be accessible with the ProROM, you will be starting fresh. Treat this as the equivalent of a format.

### History
In 1984, I developed custom firmware for the Corvus Hard Drive for Apple II computers using the parallel (flat-cable) interface card. This "ProROM" firmware gives the Corvus Hard Drive the capability of booting to Apple ProDOS, and also provides the capability to dedicate part of the hard disk drive space to DOS 3.3 Volumes similar to how the original Corvus Apple II firmware worked. Corvus never provided support for the ProDOS operating system.

This custom also provides slot independence for the Corvus interface card. The original Corvus firmware was hard coded to the I/O addresses for Slot 6.

## ProROM Features
Support for 1 ProDOS volume per hard disk drive with 0 or more 50 track DOS 3.3 volumes.
1 or 2 parallel Corvus hard drives can be attached to the controller card.
The Corvus multiplexer can be used to support up to 8 Apple II computers sharing 1 or 2 hard drives.

### How It Works
This firmware contains both a ProDOS block I/O driver, and a DOS 3.3 RWTS driver in firmware.
When booting, the hard disk drive will boot into ProDOS first.
From ProDOS, you can switch to DOS 3.3 by executing a binary DOS 3.3 binary program stored in the ProDOS volume. Both Apple DOS 3.3 and Diversi-DOS are supported.
Once in DOS, you access disk volumes by adding a ",Vnnn" to a DOS command, as was done with the original Corvus firmware.
Unlike the Corvus firmware, the DOS volumes are designed to store more files by using 50 tracks per DOS volume instead of 35.
When switching to DOS from ProDOS, DOS will attempt to run the "HELLO" program on the highest DOS volume number configured.
For example, if you configure the drive with 25 DOS volumes, switching to DOS will attemnpt to run "HELLO" on volume 25 when it boots.
To switch back to ProDOS, simply reboot the computer.

### Implementation Details
The partitioning information is stored on the first 512 byte hard disk drive block.
The ProROM makes this first block appear to ProDOS as block # -1, $FFFF
You can inspect and modify this configuration block using the ProDOS Excersiser to Read or Write block $FFFF

### Configuring the ProROM
Once you have installed the ProROM firmware into your Corvus Apple II parallel interface card, you must first configure the size of the hard disk drive, and tell the firmware how many DOS 3.3 volumes you would like on the hard drive. This is done by using the provided "ProROM 2025 Configuration Software". Please see the ["ProROM 2025 Quick Start"](https://github.com/pmontelo/savethecorvus/blob/main/Corvus%20ProROM%20Quick%20Start.pdf) document.

### Source Code
All assembly language source code was created using EDASM for ProDOS
