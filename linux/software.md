Install software from non-OS repos

# APT
# Visual Studio Code
Download the Microsoft GPG key, and convert it from OpenPGP ASCII armor format to GnuPG format, 
move it into the apt trusted keys directory, then add the VS Code repository.

``` sh
curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
mv microsoft.gpg /etc/apt/trusted.gpg.d/microsoft.gpg
echo "deb [arch=amd64] https://packages.microsoft.com/repos/vscode stable main" > /etc/apt/sources.list.d/vscode.list
 
# Update and install Visual Studio Code 
apt update && apt install code
```

# Node.js
Get node fresh from the source

``` sh
curl -sL https://deb.nodesource.com/setup_8.x | sudo -E bash -
sudo apt install -y nodejs
```

# Laverna
Download Laverna and unzip to ```/opt```, and change ownership ```chown -R root:root laverna```.
Then add the following file in /usr/share/applications

``` ini
[Desktop Entry]
Name=Laverna
Comment=Record information in Markdown
Keywords=onenote;evernote;notes;laverna;keepnote;
Exec=/opt/laverna/laverna
Icon=/home/rafael/Dropbox/Computers/Linux/icons/laverna.png
Terminal=false
Type=Application
Categories=Office;Productivity
```

# Etcher
``` sh
echo "deb https://dl.bintray.com/resin-io/debian stable etcher" | sudo tee /etc/apt/sources.list.d/etcher.list

sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 379CE192D401AB61

sudo apt-get update
sudo apt-get install etcher-electron
```

# Steam
``` sh
sudo dpkg -i steam.deb

sudo dpkg --add-architecture i386

sudo apt update

sudo apt install libc6:i386 libqt4-dbus:i386 libqt4-network:i386 libqt4-xml:i386 libqtcore4:i386 libqtgui4:i386 libqtwebkit4:i386 libstdc++6:i386 libx11-6:i386 libxext6:i386 libxss1:i386 libxv1:i386 libssl1.0.2:i386 libpulse0:i386 libasound2-plugins:i386 libgl1-mesa-glx:i386 mesa-vulkan-drivers mesa-vulkan-drivers:i386

First run may need to be done with:
LD_PRELOAD='/usr/$LIB/libstdc++.so.6' LIBGL_DRI3_DISABLE=1 steam
```

# Burpsuite
Install the linux commandline environment. Then 

``` sh
# Install elinks, or another text based browser
sudo apt install elinks

# Go to Burp Suite's site and download a copy (it's about 95 MB)
elinks https://portswigger.net/burp/communitydownload

# Save the 'Download for Linux (64-bit)', then make the file executable
chmod u+x burpsuite_community_linux_v1_7_36.sh

# Run the program
./burpsuite_community_linux_v1_7_36.sh

# Run the 
./BurpSuiteCommunity
```

Once you've done this, you'll need to install a proxy switching extension for chrome. The one I 
used is [Proxy SwitchyOmega](https://chrome.google.com/webstore/detail/proxy-switchyomega/padekgcemlokbadohgkifijomclgjgif?hl=en), 
in which you should set up two profiles:

* A 'normal' one that's set to DIRECT connection
* A 'BurpSuite' one that is set to http, with 127.0.0.1 for the server, and 8080 for the port

Once this is set up, you should be able to browse, with a bunch of warnings and broken links for 
secure webpages. The next step is to install Burp Suite's CA certificate, to get rid of these 
errors.

1. Navigate to http://127.0.0.1:8080/ (The official docs say to go to http://burp/ but this will 
   not work on ChromeOS)
2. Click on 'CA Certificate' in the top right-hand corner
3. Open ChromeOS settings, search for SSL and navigate to 'Manage Certificates'
4. Select 'DER-encoded binary, single certificatel from the file type on the bottom left, select 
   'cacert.der' and click open
5. Tick 'Trust this certificate for identifying websites' and click okay

At this point you should be set up, and able to use Burp Suite without errors. Happy hunting!

```
# Burpsuite regex to control s
.*\.domain\.com$
```







----------------------------------------------------------------------------------------------------
# RPM
----------------------------------------------------------------------------------------------------

# Xmind
Download latest Xmind from https://www.xmind.net/. Install java with 
```dnf install java-1.8.0-openjdk```. Unzip to ```/opt``` and ```chown -R root:root /opt/xmind```.

``` sh
# Create a user-specific space for Xmind to store information: 
mkdir -p ~/.config/xmind/workspace

# Remove the 32bit version
rm -rf xmind/XMind_i386

# Copy over the initial config
cp -a xmind/XMind_amd64/configuration ~/.config/xmind/
cp -a xmind/XMind_amd64/p2 ~/.config/xmind/
```

Update ```XMind.ini``` with the new directories. 
1. On line 2, change ```./configuration``` to ```@user.home/.config/xmind/configuration```
2. On line 4, change ```../workspace``` to ```@user.home/.config/xmind/workspace```

```
# Copy the fonts directory
sudo cp /opt/xmind/fonts /usr/share/fonts
rm -rf /opt/xmind/fonts
```

Then create a desktop file:
``` ini
[Desktop Entry]
Comment=Create and share mind maps
Exec=/opt/xmind/XMind_amd64/XMind %F
Path=/opt/xmind/XMind_amd64
Name=XMind
Terminal=false
Type=Application
Categories=Office;Productivity
Icon=/home/rafael/Dropbox/Computers/Linux/icons/xmind.png
```