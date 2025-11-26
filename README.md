# KDEb_NV
Easy and right way to make minimal or full KDE Plasma install on Debian with Nvidia GPU on desktop PC work correctly together with Intel IGPU 

---

### Linux Installation: 

Download and install latest Debian netinstaller, proceed your standard installing procedure until selection of additional software and desktop environment. Uncheck all DEs with space, select only standard system utilities and optional SSH server 

After boot into fresh installed linux at terminal stage login into root and install sudo:

```
apt install sudo 
```

### If non-root user was not created during Debian installation process create new one before installing desktop environment:

```
adduser newuser
```

Add created or new user into sudo group:

```
usermod -aG sudo newuser
```

 Reboot with

```
reboot
```

---

# Only for Nvidia GPUs

  
Login into your non-root account and install linux headers for proprietary nvidia driver:

```
sudo apt install linux-headers-amd64 
```

Add "contrib", "non-free" and "non-free-firmware" components to /etc/apt/sources.list:

```
sudo nano /etc/apt/sources.list  
```

Turn

```
deb http://deb.debian.org/debian/ bookworm main non-free-firmware 
deb-src http://deb.debian.org/debian/ bookworm main non-free-firmware
```

Into:

```
deb http://deb.debian.org/debian/ bookworm main contrib non-free non-free-firmware 
deb-src http://deb.debian.org/debian/ bookworm main contrib non-free non-free-firmware
```

Save and exit, update repos:

```
sudo apt update
```

Install Nvidia dirver:

```
sudo apt install nvidia-driver firmware-misc-nonfree
```

After standard message about nouveau driver conflict and successful installation of proprietary driver reboot system:

```
sudo reboot
```
### x86 support:


Install x86 support for x86 applications (Steam, Games, Davinci Resolve, etc.)

```
sudo dpkg --add-architecture i386
```

```
sudo apt update
```

```
sudo apt upgrade
```

Nvidia driver libs for x32 applications. You keep your x64 driver for x64 applications:

```
sudo apt install nvidia-driver-libs:i386
```

```
sudo reboot
```
---

### KDE installation:

Login into your non-root user, install kde-desktop, SDDM, GTK applications theme fixes and SSH if SSH server was installed during Debian installation

```
sudo apt install kde-plasma-desktop sddm breeze-gtk-theme kde-config-gtk-style kde-config-gtk-style-preview ssh
```

Comment your interfaces in /etc/network/interfaces to allow NetworkManager in Plasma control your net:

```
sudo nano /etc/network/interfaces
```

Turn

```
# The primary network interface
allow-hotplug enp7s0
iface enp7s0 inet dhcp
```
Into:

```
# The primary network interface
#allow-hotplug enp7s0
#iface enp7s0 inet dhcp
```
Reboot after successful installation of KDE plasma:

```
sudo reboot
```
You may not be able to login using Wayland, if so:

**Grub Fix**:

`sudo nano /etc/default/grub`

`GRUB_CMDLINE_LINUX_DEFAULT="quiet splash nvidia-drm.modeset=1"`

`sudo update-grub`
`sudo reboot`

**Or Modprobe Fix**:

`sudo nano /etc/modprobe.d/nvidia-drm.conf`

Add:

`options nvidia_drm modeset=1`

Then update your initramfs and reboot.

`sudo update-initramfs -u`
`sudo reboot`


After login into fresh installed desktop you need to install standard sound server on your choice:

**PulseAudio:**

```
sudo apt install pulseaudio pavucontrol pamixer
```

OR **PipeWire**:

https://wiki.debian.org/PipeWire

You can optional install standard or full package of KDE:


```
sudo apt install kde-standard
```

or

```
sudo apt install kde-full
```

Reboot:

```
sudo reboot
```

---

### Steam install:

```
sudo apt install steam-installer
```

```
sudo apt install mesa-vulkan-drivers libglx-mesa0:i386 mesa-vulkan-drivers:i386 libgl1-mesa-dri:i386
```

Open "Application Launcher", search for Steam Installer and proceed installation as usual

---

### Enable file search indexer 

Baloo file indexer is disabled by default in Debian. To enable it go to Search > File Search in system settings or execute

```
sudo balooctl enable
```

---

### If met screen tearing in DE

Create or open kwin profile:

```
sudo nano /etc/profile.d/kwin.sh
```
Add there line and save file:

```
export KWIN_TRIPLE_BUFFER=1
```
Open nvidia-settings:

```
nvidia-settings
```
Navigate to OpenGL Settings and disable "Sync to VBlank"

---

### Optional

KAccounts app for google/nextcloud/etc. accounts section in system settings

```
apt install kaccounts-integration kio-gdrive
```

#### Install from Discover:

 **Kate plugins for preview:**

https://apps.kde.org/markdownpart/

https://apps.kde.org/svgpart/

**Apps:**

https://apps.kde.org/kdenlive/

https://apps.kde.org/krita/


### Sources

https://wiki.debian.org/

https://wiki.debian.org/KDE

https://wiki.debian.org/PulseAudio

https://wiki.debian.org/PipeWire

https://wiki.debian.org/NvidiaGraphicsDrivers
