---
title: Kali Linux on the GPD P2 Max
date: 2020-06-07
layout: post
permalink: /2020/06/kali-linux-on-gpd-p2-max/
featured_image: /assets/img/logos/kalilogo.jpg
excerpt: The GPD P2 Max is a small 8.9 inch mini notebook. You don't need one, but like me you've probably bought one anyway. It lends itself well to running linux, and with Kali it offers a mobile hacking toolset that isn't underpowered, and is surprisingly usable given the form factor. Here is how to set one up.
---
## First things first
Be aware that you won't be able to get the fingerprint scanner working at the moment. For reasons highlighted [here](https://gpdsupport.com/t/linux-fingerprint-scanner-not-working/296), the current fingerprint reader isn't supported *yet*.

Before you install Linux, you need to update the firmware to a newer one that has less linux compatibility issues. As of writing, that's 0.27. Mine arrived BIOS 0.29, so I didn't touch it. You can see instructions [here](https://www.gpd.hk/gpdp2maxfirmwaredriverbios). Once the BIOS is updated, you'll need to disable secure boot. Add a password while you are there.

## Installing Kali
The touchpad & touchscreen will not work on installation, so make sure you have an external mouse if you don't want to Tab around a lot. The wifi card connected to my AP without a problem. After installation finished the mouse & touchpad worked fine. XFCE works okay if you have great eyesight (you can fix that with these [instructions](https://www.kali.org/docs//general-use/hidpi/)), but I installed Gnome. I'd recommend installing with encrypted LVM, then you can have your account auto-login so that you're only entering one password on boot.

## Update
To create a reasonable baseline you should update your kali installation and reboot.

``` sh
sudo apt update && sudo apt full-upgrade -y && sudo apt autoremove -y

# It's probably also a good time to update the locate database
sudo updatedb
```

## Enable sensors
So that the OS can tell the temperature it’s operating at, and control the fans, you will need to install lm-sensors, and activate them. It will only pick up `coretemp`, and you should answer 'yes' to add it to `/etc/modules`.

``` sh
sudo apt install -y lm-sensors
sudo sensors-detect
sudo systemctl enable lm-sensors
```

## Enable Bluetooth
I've always found bluetooth a bit touchy, but it does work on this device. First you need to enable and start the bluetooth service with be below commands, then use the bluetooth panel in settings to pair up with your devices. Theoretically you shouldn't need to restart before doing so, but I do to rule out pairing failure due to any other programs.

``` sh
sudo systemctl start bluetooth
sudo systemctl enable bluetooth
```


## Save Power
First, install the following packages tlp tlp-rdw powertop. The first two are a for a power tuning daemon for laptops. You can activate it by enabling the service:

``` sh
sudo apt install -y tlp tlp-rdw
sudo systemctl start tlp
sudo systemctl enable tlp
```

## Prefer RAM to Swap
Usage of the swap partition is governed by the swappiness value, which defaults to 60. You can configure the system to use RAM in preference, which makes sense on the 16GB model (and probably the 8 GB model too).

``` sh
# Confirm current swappiness
cat /proc/sys/vm/swappiness

# Edit /etc/sysctl.conf and add the following value
vm.swappiness = 10
```


## Fix smartd
smartd monitors your SMART capable devices for temperature and errors. Unfortunately, NVMe support is still experimental for smartd, so it doesn’t scan for it by default, and fails on boot. You can fix this by telling it to scan for NVMe drives by adding -d nvme to /etc/smartd.conf in the DEVICESCAN line. Make the first uncommented line in /etc/smartd.conf look like this:

```
DEVICESCAN -d removable -d nvme -n standby -m root -M exec /usr/share/smartmontools/smartd-runner
```

# Setup Software
## Get NTP Working


## Enable Printing (if you want it)
If printing is a thing you think you will want to do, then you need to install the packages and set it up. 

``` sh
apt install -y cups
systemctl start cups
systemctl enable cups
```
Then add your everyday user to the 'lpadmin' group to enable you to administer printers without going via root
``` sh
gpasswd -a username lpadmin
```

# Security
If you are paranoid about your security, Kali is probably not the distro for you to have as a host. Maybe try something with SELinux and virtualize kali... but who has time for that? Realistically, a small laptop like this will be just fine with some basic precautions (depending on your threat model). Consider putting in place the following:

## Basics
1. Disable USB & Network boot 
2. Set a BIOS Password
3. Set a GRUB2 Password (below)

``` sh
# Create a Grub password:
grub2-mkpasswd-pbkdf2

# Edit /etc/grub.d/10_linux and add the following:
set superusers="Username"
password_pbkdf2 Username 'Paste the generated code from above'

# While you are in this area, edit /etc/default/grub and replace GRUB_TIMEOUT with a lower number (like 0) so that you can boot faster. You can still easily interrupt the grub boot process if you are prepared.

# The update the grub config
update-grub
```

## Enable a Firewall
The easiest choice for this is to use UFW, but you have other options if you want. The below commands allow ssh in by default, and nothing else. Remember to set SSH to authenticate via keys only.

``` sh
sudo apt install -y ufw gufw
sudo ufw enable
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw allow ssh
```

## Change the default SSH Keys
The default ssh keys that kali installs should be changed to new random ones

``` sh
cd /etc/ssh
sudo mkdir bak # <- Always back up
sudo mv ssh_host_* /etc/ssh/bak
sudo dpkg-reconfigure openssh-server # Ignore 'static unit' error messages
```

## Encrypt your Home directory
Full disk encryption with LUKS is the most secure approach, but if you are happy with just encrypting the files in your home directory (as I am) ecryptfs offers a more seamless solution.

``` sh
sudo apt install ecryptfs-utils
 
# Add the appropriate kernel module
sudo modprobe ecryptfs
 
# Then add 'ecryptfs' to this file to make it persistant through reboots
sudo vim /etc/modules-load.d/modules.conf
 
# Encrypt the home folder
ecryptfs-migrate-home -u <username>
 
# And then log in as that user, BEFORE REBOOTING
# If you were using dropbox, you'll need to re-login
```
Once this is done, you should generate a key for recovery, by running `ecryptfs-unwrap-passphrase` as the encrypted user.

For complete protection, if you can live without hibernate/resume capabilities, you can encrypt your swap space (you’ll still keep suspend/resume) by running `ecryptfs-setup-swap`.

Note: While you can set this up for the root user, do not do this, and make sure you only update software from the account that has had it’s files encrypted. Otherwise, when updates need to make changes to your .config directory, they won’t be able to, and you may be left with an unusable account. I learnt this the hard way. For safety, I also recommend adding the following to your root’s .bashrc:

``` sh
alias apt="echo \"Are you about to break something? If you're SURE, use apt-unsafe\""
alias apt-unsafe="apt"
```

# You're done
At this stage you should be ready to install your regular tools, and do all the setup appropriate to your system. It's worth confirming everything is working by checking `dmesg` for errors, and `systemctl list-units --state=failed` for failed services. If you find anything, google it and fix it.

I use a script to maintain the software on all my kali installs; with some editing, you may find it useful too: https://github.com/rafaelh/update-kali.