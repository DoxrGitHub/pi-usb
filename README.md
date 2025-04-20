# pi-usb
Use your Pi as a bootable/mountable storage USB

## Why?
If you have multiple bootable images and don't want to keep flashing a USB drive to swap through them, you can use this. Or if you want to use pi-usb to spoof a (or multiple) normal, regular thumbdrive(s), it is also possible (see below). For example, I have multiple ChromeOS shims and recoveries, and I can't use Ventoy as the Chromebook won't accept that; I can use this to serve the recovery/shim without needing to flash to a USB.

## Support

| Model                     | Works? | Notes                                                |
|---------------------------|--------|------------------------------------------------------|
| Raspberry Pi Zero          | ⚠️     | Has a micro-USB OTG port, but you'd have to figure out SSH/auto-setup with this |
| Pi Zero W / WH             | ✅     | Perfect for this                                      |
| Pi Zero 2 W                | ✅     | Used this for testing                                |
| Pi 4 (via USB-C)           | ⚠️     | Has OTG on USB-C, not tested though                  |
| Pi 3 A+ / B / B+           | ❌     | No OTG port support for gadget mode                  |
| Pi 2 / Pi 1 / Pi B+        | ❌     | No OTG support                                       |
| Pi Compute Modules         | ???    | I don't know maybe with some manual hacking          |
| Raspberry Pi 400           | ❌     | No OTG/gadget support                                |
| Pi 5                       | ❌⚠️   | Theoretically possible but literally not worth it    |


Basically if you have a Pi Zero you're good

## Install Script

To do

## Run Script

to do

### Spoof a Writable Thumbdrive

If you want to create a writable USB storage space to use normally, run these commands to create a drive file

```
dd if=/dev/zero of=USB_FILE_NAME.bin bs=1M count=8192
mkfs.vfat USB_FILE_NAME.bin
```

Replace USB_FILE_NAME with the file name, 8192 with the MB number of the drive (the given commands create a 8GB file), and you can replace the second command with something like mkfs.ntfs or ext4 to format the drive file.
