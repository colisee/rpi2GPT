# rpi-img2GPT
Flash a raspios disk image to a GPT-partitioned device

## Usage:
    sudo rpi-img2GPT [options] --image <image_file> --device <device_name>

    Options:
    --boot-size <size>
    --root-size <size>
    --home-size <size>
    --root-size <size>

Notes:
- You can use the suffix (K, M, G, T) as in 64G
- A size of "0" means the remaining disk space
- By default, the boot partition occupies 64 Mb and the root partitions takes
the remaining device space

## Example:
```
sudo rpi-img2GPT \
	--boot-size 128M \
	--root-size 64G \
	--home-size 16G \
	--var-size 0 \
	--image 2021-05-07-raspios-buster-armhf-lite.img \
	--device /dev/sdc
```

## What the command does
This command will:
- Erase the specified device (usually a hard disk)
- Create 2 partitions: boot (default=64MB) and root (default=remaining space)
- Create up to 2 additional partitions (home and var) if their size is
 specified
- Copy the content of the disk image to the device
- Change the file /boot/cmdline.txt to:
  - Define the new root mount (switching to PARTLABEL from PARTUUID)
  - Remove the init and fsck arguments
- Change the file /etc/fstab to update the mount points
