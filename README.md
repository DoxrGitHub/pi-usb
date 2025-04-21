# Pi-USB

Use your Pi as a bootable/mountable storage USB

## Why?
If you have multiple bootable images and don't want to keep flashing a USB drive to swap through them, you can use this. For example, I have multiple ChromeOS shims and recoveries, and I can't use Ventoy as the Chromebook won't accept that; I can use this to serve the recovery/shim without needing to flash to a USB. Or if you want to use Pi-USB to spoof a (or multiple) normal, regular thumbdrive(s), it is also possible (see below). 

## Support

| Model                     | Works? | Notes                                                |
|---------------------------|--------|------------------------------------------------------|
| Raspberry Pi Zero          | ✅⚠️   | Has a micro-USB OTG port, but just make sure to use auto-setup prior to usage with the host* |
| Pi Zero W / WH             | ✅     | Perfect for this                                     |
| Pi Zero 2 W                | ✅     | Used this for testing                                |
| Pi 4 (via USB-C)           | ✅⚠️   | Has OTG on USB-C, needs some extra work**            |
| Pi 3 A+                    | ✅     | The Micro-USB port          supports USB OTG         |
| Pi 3 B / B+                | ❌     | No OTG port support for gadget mode                  |
| Pi 2 / Pi 1 / Pi B+        | ❌     | No OTG support                                       |
| Pi Compute Modules         | ❌⚠️     | Theoretically possible in some cases but unsure                    |
| Raspberry Pi 400           | ❌     | No OTG/gadget support                                |
| Pi 5                       | ❌     | No OTG/gadget support                                |


*Basically, if you have a Pi Zero, you're good; however, if you're using Pi-USB without wifi and/or a headless setup/SSH already set up, then in you should look into using auto-setup for purposes like recovery images, when the host is unable to provide the Pi Zero for Ethernet.

**You need to connect the Pi 4 to the host through the USB-C port, which switches it to OTG mode. However, this means that the Pi 4 can't draw power from the host anymore. To solve this, you'll need a USB-C Y cable or splitter. The data side of the Y cable connects to the host, while the power side should be connected to a separate power source. This way, the Pi 4 can be connected to the host for data and also be powered simultaneously.

## How do I use it?

Keep in mind: to use this, you need to plug in the Pi to the host using the USB port (this is the closest one on the right of the micro HDMI port), not the PWR IN port. This means that you cannot use Ethernet to start SSH with your Pi, if the host is not online (which is usually the case when you need to recover your system with a recovery image); to mitigate this, either set up WiFi and start SSH on boot (possible with a headless Pi Zero W setup), or set up auto-start for an image while you are still able to SSH/run commands on the machine.

This script was made for Debian/RPI OS, supports older and newer versions; you may need to modify Pi-USB to support other distributions like Arch or Alpine.

### Install Script

You *need* root access/sudo! 

1. Run these commands (probably through SSH) to install the script to `~/pi-usb/`

```sh
cd
mkdir -p pi-usb
curl https://raw.githubusercontent.com/DoxrGitHub/pi-usb/refs/heads/main/pi-usb -o pi-usb
cd pi-usb
chmod +x pi-usb
```

This will create the `pi-usb` utility. When you're ready (this will require you to reboot to take effect), run the setup. You cannot use Pi-USB without running the setup, as the setup sets up USB OTG.

```sh
cd ~/pi-usb/
./pi-usb --setup
```

When prompted, enter `y`, and you will reboot. Now, you can delete `~/pi-usb/pi-usb`, as the setup process now lets you run Pi-USB as a normal command.

### Documentation

```
Usage:
  sudo pi-usb -f /path/to/image.img [--ro|--rw] [--removable|--non-removable]
  sudo pi-usb --unmount
  sudo pi-usb --status
  sudo pi-usb --setup
  sudo pi-usb --auto-setup /path/to/image.img [--ro|--rw] [--removable|--non-removable]
  sudo pi-usb --auto-disable

Options:
  -f, --file PATH        Path to the image file to mount
  --ro                   Mount image as read-only (default)
  --rw                   Mount image as read-write
  --removable            Mount image as removable media (default)
  --non-removable        Mount image as non-removable media
  --unmount              Unmount any active USB storage
  --status               Show current status
  --setup                Configure Pi for USB mass storage mode
  --auto-setup PATH      Configure automatic mounting of specified image on boot
  --auto-disable         Disable automatic mounting on boot
  --auto-mount           Mount image defined in auto-mount config (used by systemd)
  -h, --help             Show the help message

Examples:
  sudo pi-usb -f /path/to/usb_backup.img --rw --non-removable
  sudo pi-usb --auto-setup chromeos_recovery.bin --ro # You don't need to pass the full path
```

### Extra: Create a Writable, Empty Drive File

If you want to create a writable USB storage space to use normally, run these commands to create a drive file

```
dd if=/dev/zero of=USB_FILE_NAME.bin bs=1M count=8192
mkfs.vfat USB_FILE_NAME.bin
```

Replace USB_FILE_NAME with the file name, 8192 with the MiB number of the drive (the given commands create a 8GB file), and you can replace the second command with something like mkfs.ntfs or ext4 to format the drive file. Pass this file to Pi-USB, and use it as a drive.
