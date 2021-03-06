# Crouton Quick Reference

These steps are specificly for my own quick reference, but they may also be useful for others as well.

## Developer Mode

1. Press **power** while holding down **esc+refresh**.

1. Press **ctrl+d** at Recovery Mode screen.

1. Press **enter** to confirm entering developer mode.

1. Press **ctrl+d** at OS Verification Off Screen

    * This will show up at every boot.  Press **ctrl+d** to boot from now on.

## Be Secure

After enabling developer mode, it is pretty important to enable some additional security mechanisms.

### Set a password for the default user (chronos)

	sudo chromeos-setdevpasswd

### Enable signed boot verification

	sudo crossystem dev_boot_signed_only=1

### Restrict Chrome OS Access

1. Navigate to **Settings** -> **Manage other users...**

1. Uncheck **Enable Guest browsing**

1. Check **Restrict sign-in to the following users**

## Installation

### Base

	wget 'https://goo.gl/fd3zc' -O ~/Downloads/crouton
	sudo sh -e ~/Downloads/crouton -n crouton -r xenial -t core,cli-extra,x11,chrome,keyboard

### GUI (Xfce)

	sudo sh ~/Downloads/crouton -u -n crouton -t xfce

### Chrome Extension

1. Install [crouton integration](https://chrome.google.com/webstore/detail/crouton-integration/gcpneefbbnfalgjniomfjknbcgkbijom).

1. Install xiwi and extension packages

		sudo sh ~/Downloads/crouton -u -n crouton -t xiwi,extension

	* `xiwi` - X.org X11 backend running **unaccelerated** in a Chromium OS window. (not required)
	* `extension` - Clipboard synchronization and URL handling with Chromium OS. (recommended)

## Fix-Ups

### Fix touchpad in Xorg

This command will fix touchpad support in Xorg for the HP Chromebook 13 and should be run outside the chroot.

	sudo sed -i.orig -e 's/butterfly\*|falco\*|sentry\*/butterfly\*|falco\*|sentry\*|chell\*/' /mnt/stateful_partition/crouton/chroots/crouton/usr/local/bin/croutonxinitrc-wrapper

### Modify SSH Client Config

Disable `HashKnownHosts` so that ssh does not hash host names and addresses when they are added to `~/.ssh/known_hosts`.  This command should be run inside the chroot (`sudo enter-chroot`).

	sudo sed -i.orig -e 's/\(\s*HashKnownHosts\s*\).*$/\1no/' /etc/ssh/ssh_config

### Fix Audio / MIDI Permissions

This command should be run inside the chroot (`sudo enter-chroot`).

	sudo usermod -a -G hwaudio uncon

### Remove Useless Packages

This command should be run inside the chroot (`sudo enter-chroot`).

	sudo apt-get purge xscreensaver netsurf-{common,gtk}

## Additional Software

These commands should be run inside the chroot (`sudo enter-chroot`).

1. Install prerequisites

		sudo apt-get install software-properties-common python-software-properties

### Wireshark

	sudo add-apt-repository ppa:wireshark-dev/stable
	sudo apt-get update
	sudo apt-get install wireshark

### Minecraft

	sudo add-apt-repository ppa:minecraft-installer-peeps/minecraft-installer
	sudo apt-get update
	sudo apt-get install minecraft-installer

### Other Useful Stuff

	sudo apt-get install curl dnsutils tmux vim zsh git

## Startup

### With GUI (Xfce)

	sudo startxfce4 -b

### With CLI
	
	sudo enter-chroot

## Maintenance

### Update

	sudo sh ~/Downloads/crouton -u -n crouton

### Backup

	sudo edit-chroot -b crouton

### Delete

	sudo edit-chroot -d crouton

### Restore

	sudo sh -e ~/Downloads/crouton -r xenial -f ~/Downloads/crouton.tar.gz
