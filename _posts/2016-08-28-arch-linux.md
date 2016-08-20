Descargar iso

https://www.archlinux.org/download/

It is recommended to verify the image signature before use, especially when downloading from an HTTP mirror, where downloads are generally prone to be intercepted to serve malicious images.
On a system with GnuPG installed, do this by downloading the PGP signature (under Checksums) to the ISO directory, and verifying it with gpg --keyserver-options auto-key-retrieve --verify archlinux-<version>-dual.iso.sig.
Alternatively, run pacman-key -v archlinux-<version>-dual.iso.sig from an existing Arch Linux installation.

bajar firma. La firma y la iso tienen q estar en el mismo directorio

		$ pacman-key -v archlinux-2016.08.01-dual.iso.sig 
		==> Checking archlinux-2016.08.01-dual.iso.sig...
		gpg: assuming signed data in 'archlinux-2016.08.01-dual.iso'
		gpg: Signature made Mon 01 Aug 2016 11:43:08 AM COT using RSA key ID 7F2D434B9741E8AC
		gpg: Note: trustdb not writable
		gpg: Good signature from "Pierre Schmitz <pierre@archlinux.de>" [full]

Quemar USB

		$ sudo fdisk -l
		$ sudo dd bs=4M if=archlinux-2016.08.01-dual.iso of=/dev/sdb status=progress && sync

Arrancar PC desde USB

F12 -> Boot Arch Linux x86_64

Q
https://wiki.archlinux.org/index.php/Arch_boot_process




## Conectar a Internet
## Bajar recordmydesktop
## Iniciar grabación


ls /usr/share/kbd/keymaps/**/*.map.gz
loadkeys la-latin1


ls /usr/share/kbd/consolefonts/
$ showconsolefont
The setfont utility may be used to temporarily change the font, so that the user can consider its permanent use. Just pass the name of the font (they are located in /usr/share/kbd/consolefonts/). For example:
$ setfont lat2-16 -m 8859-2
Note that the font name is case-sensitive, so type it exactly as you see it. If the newly changed font is not suitable, a return to the default font with the following command (even if the console display is totally unreadable, this command will still work, just type the command "blindly"):
$ setfont
Note: setfont only works on the console currently being used. Any other consoles, active or inactive, remain unaffected.
Persistent configuration
The FONT variable in /etc/vconsole.conf is used to set the font at boot, persistently for all consoles. See man 5 vconsole.conf for details.
For displaying characters such as Č, ž, đ, š or Ł, ę, ą, ś using the font lat2-16.psfu.gz:
/etc/vconsole.conf
...
FONT=lat2-16
FONT_MAP=8859-2

		# setfont lat9w-16

## Explicar linux filesystem

## Conectar a Internet

Revisamos interfaces

		# ip ad

Detenemos servicio dhcp en ethernet si activo

		# systemctl status dhcpcd@enp0s25.service
		# systemctl stop dhcpcd@enp0s25.service

wifi

		# wifi-menu -o wlp2s0

The resulting configuration file is stored in /etc/netctl. For networks which require both a username and password, see WPA2 Enterprise#netctl.
Other
Several example profiles, such as for configuring a static IP address, are available. Copy the required one to /etc/netctl, for example ethernet-static:
		# cp /etc/netctl/examples/ethernet-static /etc/netctl
Adjust the copy as needed, and enable it:
		# netctl start ethernet-static





Update the system clock
Use systemd-timesyncd to ensure that your system clock is accurate. To start it:
		# timedatectl set-ntp true
To check the service status, use timedatectl status.Update the system clock
Use systemd-timesyncd to ensure that your system clock is accurate. To start it:
		# timedatectl set-ntp true
To check the service status, use timedatectl status.

Time zone

To check the current zone defined for the system:
		$ timedatectl
To list available zones:
		$ timedatectl list-timezones
To change your time zone:
		# timedatectl set-timezone Zone/SubZone
Example:
		# timedatectl set-timezone Canada/Eastern






# Discos

https://wiki.archlinux.org/index.php/Disk_encryption

		# lsblk

Not all devices listed are viable mediums for installation; results ending in rom, loop or airoot can be ignored.thunderbird enigmail keys storage
Note: In the sections below, the sdxy notation will be used (x for the device, y for an existing partition

Partitioning a hard drive divides the available space into sections that can be accessed independently. The required information is stored in a partition table using a format such as MBR or GPT. Existing tables can be printed with parted /dev/sdx print or fdisk -l /dev/sdx.

particionar con cfdisk (MBR)

swap -> free

MBR/BIOS example layout
Mount point	Partition	Partition type	Bootable flag	Suggested size
[SWAP]	/dev/sdx1	Linux swap	No	More than 512 MiB
/	/dev/sdx2	Linux	Yes	20GB
/home	/dev/sdx3	Linux	Yes	Remainder of the device


		# lsblk /dev/sdx
With the exceptions noted below, it is recommended to use the ext4 file system:
		# mkfs.ext4 /dev/sdxy
If a swap partition was created, it must be set up and activated with:
		# mkswap /dev/sdxy
		# swapon /dev/sdxy



Montar las particiones

Mount the root partition to the /mnt directory of the live system:
		# mount /dev/sdxy /mnt
Remaining partitions except swap may be mounted in any order, after creating the respective mount points. For example, when using a /boot partition:
		# mkdir -p /mnt/boot
		# mount /dev/sdxy /mnt/boot

# Instalar

mirrors
The higher a mirror is placed in the list, the more priority it is given when downloading a package. You may want to edit the file accordingly, and move the geographically closest mirrors to the top of the list, although other criteria should be taken into account.
The pacstrap tool used in the next step also installs a copy of the file to the new system, so it is worth getting right.

paquetes

		# pacstrap -i /mnt base base-devel





# Configuración

		# genfstab -U /mnt >> /mnt/etc/fstab
		# arch-chroot /mnt /bin/bash



Locale
The Locale defines which language the system uses, and other regional considerations such as currency denomination, numerology, and character sets.
Uncomment en_US.UTF-8 UTF-8 in /etc/locale.gen, as well as other needed localisations. Save the file, and generate the new locales:
		

Create /etc/locale.conf, where en_US.UTF-8 refers to the first column of an uncommented entry in /etc/locale.gen:

		/etc/locale.conf
		LANG=en_US.UTF-8

If you set the keyboard layout, make the changes persistent in /etc/vconsole.conf. For example, if de-latin1 was set with loadkeys, and lat9w-16 with setfont, assign the KEYMAP and FONT variables accordingly:
/etc/vconsole.conf

		KEYMAP=de-latin1
		FONT=lat9w-16

		# locale-gen

		
Time
Select a time zone:
		# tzselect
		# ln -s /usr/share/zoneinfo/Zone/SubZone /etc/localtime
		# hwclock --systohc --utc

Initramfs
		# mkinitcpio -p linux



Boot loader

It is the first piece of software started by the BIOS or UEFI. It is responsible for loading the kernel with the wanted kernel parameters, and initial RAM disk before initiating the boot process.

		# pacman -S grub
		# grub-install --target=i386-pc /dev/sdx
		# grub-mkconfig -o /boot/grub/grub.cfg

Troubles 
http://pissedoffadmins.com/general/usrsbingrub2-bios-setup-warning-sector-32-is-already-in-use-by-the-program-flexnet-avoiding-it-this-software-may-cause-boot-or-other-problems-in-future-please-ask-its-authors-not-to-store.html

https://ubuntuforums.org/showthread.php?t=1661254&page=2

		# dd if=/dev/zero of=/dev/sda bs=512 count=1 seek=32



RED 

Several example profiles, such as for configuring a static IP address, are available. Copy the required one to /etc/netctl, for example ethernet-static:
# cp /etc/netctl/examples/ethernet-static /etc/netctl
Adjust the copy as needed, and enable it:
# netctl start ethernet-static


/etc/hostname
myhostname



It is recommended to append the same host name to /etc/hosts, for example:
/etc/hosts
127.0.0.1	localhost.localdomain	localhost	 myhostname
::1		localhost.localdomain	localhost	 myhostname

Install the iw, wpa_supplicant, and (for wifi-menu) dialog packages:





		# passwd


		# exit
		# umount -R /mnt
		If the partition is "busy", you can find the cause with fuser. Reboot the computer.
		# reboot


Your new Arch Linux base system is now a functional GNU/Linux environment ready to be built into whatever you wish or require for your purposesthunderbird enigmail keys storage
