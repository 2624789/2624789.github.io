awesome is a highly configurable, next generation framework window manager for X. It is very fast, extensible and licensed under the GNU GPLv2 license. It is primarily targeted at power users, developers and any people dealing with every day computing tasks and who want to have fine-grained control on its graphical environment.

Prior to installing a window manager, a functional X server installation is required.

# XORG

The X.Org project provides an open source implementation of the X Window System

First, identify your card:
		$ lspci | grep -e VGA -e 3D

		# pacman -S xorg-server 
			mesa-libgl (libgl) 
			xf86-input-libinput (input driver)
			xf86-video-ati
			xorg-xinit
			xterm

/etc/X11/xorg.conf.d/00-keyboard.conf
Section "InputClass"
        Identifier "system-keyboard"
        MatchIsKeyboard "on"
        Option "XkbLayout" "cz,us"
        Option "XkbModel" "pc104"
        Option "XkbVariant" ",dvorak"
        Option "XkbOptions" "grp:alt_shift_toggle"
EndSection


		# pacman -S awesome
		# echo exec awesome > .xinitrc












# Sound

pacman -S alsa-utils
alsamixer