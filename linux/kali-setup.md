## Add Debian Unstable Packages
``` sh
echo "deb http://ftp.debian.org/debian unstable main contrib non-free" > /etc/apt/sources.list.d/debian.list
```

## Everyday user
Some programs (VLC, Google Chrome, Visual Studio Code, etc.) object to being run as root, and I want to use different programs depending on what I’m doing, so I create a normal user for daily use.

```
adduser -m rafael -s /bin/bash 
gpasswd -a rafael sudo
gpasswd -a rafael systemd-journal
```

## Fans
So that the OS can tell the temperature it’s operating at, and control the fans, you will need to install lm-sensors, and activate them

```
apt install lm-sensors
sensors-detect
```

When sensors-detect asks if you want to make changes to /etc/modules automatically, say yes. Then make sure it loads on next boot:

```
systemctl enable lm-sensors
```

## Fix smartd
smartd monitors your SMART capable devices for temperature and errors. Unfortunately, NVMe support is still experimental for smartd, so it doesn’t scan for it by default, and fails on boot. You can fix this by telling it to scan for NVMe drives by adding -d nvme to /etc/smartd.conf in the DEVICESCAN line. Make the first uncommented line in /etc/smartd.conf look like this:

```
DEVICESCAN -d removable -d nvme -n standby -m root -M exec /usr/share/smartmontools/smartd-runner
```

## Touchpad/Touchscreen Gestures
To get pinch, zoom, and other gestures working, we need to install 
[libinput-gestures](https://github.com/bulletmark/libinput-gestures), which has some dependencies, and requires a config file.

```
#Install Dependencies
apt install libinput-tools xdotool wmctrl

#Add your everyday user to the 'input' group
gpasswd -a rafael input

#Build and Install libinput-gestures
git clone http://github.com/bulletmark/libinput-gestures
cd libinput-gestures
sudo ./libinput-gestures-setup install

#You will need to log your everyday user out and in if you are using it, so that the 'input' group is recognised.
```

Then you will need to switch to your everyday user account and create the following config file in  .config/libinput-gestures.conf :

```
 #~/.config/libinput-gestures.conf
 
# Go back/forward in chrome
gesture: swipe right 3 xdotool key Alt+Left
gesture: swipe left 3 xdotool key Alt+Right
 
# Zoom in / Zoom out
gesture: pinch out xdotool key Ctrl+plus
gesture: pinch in xdotool key Ctrl+minus
 
# Switch between desktops
gesture: swipe up 3 xdotool set_desktop --relative 1
gesture: swipe down 3 xdotool set_desktop --relative -- -1
```

Then, as your everyday user account:

```
libinput-gestures-setup start
libinput-gestures-setup autostart
```


## Enable Printing
This requires the cups service to be installed and started:

```
apt install cups
systemctl start cups.service
systemctl enable cups
```
Then add your everyday user to the 'lpadmin' group to enable you to administer printers without going via root
```
gpasswd -a rafael lpadmin
```

## Get rid of the On Screen Keyboard
Install this Gnome Extension: [block-caribou](https://extensions.gnome.org/extension/1326/block-caribou/). This is a bug, and you won’t need the extension permanently.

## Save Power
First, install the following packages  tlp tlp-rdw powertop . The first two are a for a power tuning daemon for laptops. You can activate it by enabling the service:

```
systemctl enable tlp.service
```

## Bluetooth
First step is to install bluetooth support with apt install bluetooth and rebooting. According to this post, you also need to download the windows firmware, and copy it into /lib/firmware/brcm . Enable the bluetooth service and reboot.
```
systemctl start bluetooth 
systemctl enable bluetooth
```

## Unstable Packages
``` sh
echo "deb http://ftp.debian.org/debian unstable main contrib non-free" > /etc/apt/sources.list.d/debian.list
echo "deb http://deb.debian.org/debian experimental main" >> /etc/apt/sources.list.d/debian.list
apt update
```

## Mouse Buttons
xinput list (to get an id)
xinput set-button-map id 3 2 1

Then set it as a startup script by creating a .service file in /etc/systemd/system

```
[Unit]
Description=Spark service

[Service]
ExecStart=/path/to/spark/sbin/start-all.sh

[Install]
WantedBy=multi-user.target
```

Then create the script, make it executable, and then issue the systemctl start and enable commands

--------------------------------------------------------------------

GRUB Password
You'll want to create the password using the following command, otherwise it will be stored in plaintext for all to see:

grub-mkpasswd-pbkdf2
Paste the encrypted long string into the file /etc/grub.d/40_custom together with the set superusers command. Remember to keep the commented lines at the beginning:

set superusers="root"
password_pbkdf2 root grub.pbkdf2.sha512.10000.9CA4611006FE96BC77A...

# Encrypt Home Directory
As root, install the packages needed
``` sh
apt install ecryptfs-utils
 
# Add the appropriate kernel module
modprobe ecryptfs
 
# Then add 'ecryptfs' to this file to make it persistant through reboots
vim /etc/modules-load.d/modules.conf
 
# Encrypt the home folder
ecryptfs-migrate-home -u <username>
 
# And then log in as that user, BEFORE REBOOTING
# If you were using dropbox, you'll need to re-login
```
Once this is done, you should generate a key for recovery, by running  ecryptfs-unwrap-passphrase as the encrypted user.

For complete protection, if you can live without hibernate/resume capabilities, you can encrypt your swap space (you’ll still keep suspend/resume) by running  ecryptfs-setup-swap.

Note: While you can set this up for the root user, do not do this, and make sure you only update software from the account that has had it’s files encrypted. Otherwise, when updates need to make changes to your .config directory, they won’t be able to, and you may be left with an unusable account. I learnt this the hard way. For safety, I also recommend adding the following to your root’s .bashrc:

alias apt="echo \"Are you about to break something? If you're SURE, use apt-unsafe\""
alias apt-unsafe="apt"
Firewall (UFW)
apt install ufw gufw
ufw enable
ufw default deny incoming
ufw default allow outgoing
ufw allow ssh

# Confirm it's all working
ufw status verbose
Allow Port Ranges
Port ranges may also be specified, a simple example for tcp would be:

ufw allow 1000:2000/tcp

# and for udp:

ufw allow 1000:2000/udp

# An IP address may also be used:

ufw allow from 111.222.333.444
Deleting Rules
Rules may be deleted with the following command:

ufw delete allow ssh



## Install Snaps

```bash
apt update
apt install snapd
systemctl enable --now snapd apparmor

# Test the installation:
snap install hello-world
> hello-world 6.3 from Canonical✓ installed
hello-world
> Hello World!

# Install the Snap store
sudo snap install snap-store

```

