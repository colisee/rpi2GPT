# rpi-img2GPT
Flash a raspios disk image to a GPT-partitioned device

## Usage:
```
sudo rpi-img2GPT [options] --image=image-file --device=device-name

Options:
  --boot-size=size-of-boot-partition
  --root-size=size-of-root-partition
  --home-size=size-of-home-partition
  --root-size=size-of-var-partition
  --enable-ssh
  --quiet
```

Notes:
- You can use the suffix (K, M, G, T) as in 64G when specifying sizes
- A size of 0 means the remaining disk space
- By default, the boot partition occupies 64 Mb and the root partitions takes
the remaining device space

## Example:
```
sudo rpi-img2GPT \
	--image 2021-05-07-raspios-buster-armhf-lite.img \
	--device /dev/sdc
	--boot-size 128M \
	--root-size 64G \
	--home-size 16G \
	--var-size 0 \
	--enable-ssh
```

## What the command does
This command will:
- Erase the specified device (usually a hard disk) and create a GPT table
- Create 2 partitions: boot (default=64MB) and root (default=remaining space)
- Create up to 2 additional partitions (home and var) if their size is
 specified
- Copy the content of the disk image to the device
- Change the file /boot/cmdline.txt to:
  - Define the new root mount (switching to PARTLABEL from PARTUUID)
  - Remove the init and fsck arguments
- Change the file /etc/fstab to update the mount points
- Add an empty file /boot/ssh if the argument --enable-ssh was specified
