# rpi-img2GPT
Copy a raspiOS disk image to a GPT-partitioned device (eg. hard disk)

## Usage:
```
sudo rpi-img2GPT -d|--device device_name -i|--image rpi_disk_image
```

Example:

sudo rpi-img2GPT --device /dev/sdc --image 2021-05-07-raspios-buster-armhf-lite.img

## What the command does
This command will:
- Erase the specified device (usually a hard disk)
- Partition the device with the GPT scheme and create 2 partitions - boot (64MB) and root (the rest of the device)
- Copy the content of the disk image to the device
- Change the file boot/config.txt to define a 2 x 5-second-delay to run the bootloader and kernel to leave enough time for the device to spin
- Change the file boot/cmdline.txt to define the new root mount (switching in the process to PARTLABEL from PARTUUID) and remove the call to the initial root partition resize
- Change the file root/etc/fstab to define the 2 mount points - /boot and / - (switching in the process to PARTLABEL from PARTUUID)