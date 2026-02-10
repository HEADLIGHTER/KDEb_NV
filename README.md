# KDEb13_NV
Easy and right way to make minimal or full KDE Plasma install on Debian with Nvidia GPU on desktop PC work correctly together with Intel IGPU 

---

### Linux Installation: 

Download and install latest Debian 13 netinstaller, proceed your standard installing procedure until selection of additional software and desktop environment. Uncheck all DEs with space, select only standard system utilities and optional SSH server 

After boot into fresh installed linux at terminal stage login into root and install sudo if password for root was set during installation:

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

### SecureBoot
According Debian Wiki:
> If you have SecureBoot enabled, you need to enroll your machine owner's key (MOK) to use DKMS modules. Detailed instructions are available
[here](https://wiki.debian.org/SecureBoot#dkms). It's recommended to do this before installing nvidia-driver so that you do not have to rebuild the kernel modules.

## Driver install
Login into your non-root account and install linux headers for proprietary nvidia driver:

```
sudo apt install linux-headers-generic 
```

Add "contrib", "non-free" and "non-free-firmware" components to /etc/apt/sources.list:

```
sudo nano /etc/apt/sources.list  
```

clear this file and paste

```
deb http://deb.debian.org/debian/ trixie main contrib non-free non-free-firmware

deb http://security.debian.org/debian-security/ trixie-security contrib non-free main non-free-firmware

deb http://deb.debian.org/debian/ trixie-updates non-free-firmware non-free contrib main

```

Save and exit, update repos:


### x86 support:


Install x86 support for x86 applications (Steam, Wine, Games, Davinci Resolve, etc.)

```
sudo dpkg --add-architecture i386
```

```
sudo apt update
```

Install Nvidia dirver:

### Optional!
For GPU and it's recommended/supported driver detection use nvidia-detect:

```
sudo apt install nvidia-detect
sudo nvidia-detect
```


```
sudo apt install nvidia-kernel-dkms nvidia-driver
```

After standard message about nouveau driver conflict and successful installation of proprietary driver reboot system:

You can verify dkms with:

```
sudo dkms status | grep nvidia
```

---

# KDE&Wayland installation:


Login into your non-root user, install kde-desktop, SDDM, GTK applications theme fixes and SSH if SSH server was installed during Debian installation

> Make decision about your KDE installation: 
wiki.debian.org/KDE#Installation
> (!) Watch out for recommended packages (that is, optional package dependencies)!
They are installed by default, but you might not want them.
apt option --no-install-recommends and aptitude option --without-recommends can help with this.

 Replace kde-plasma-desktop with kde-standard, kde-full or task-kde-desktop:
```
sudo apt install plasma-workspace-wayland kde-plasma-desktop sddm breeze-gtk-theme kde-config-gtk-style kde-config-gtk-style-preview kde-config-screenlocker plymouth plymouth-themes plymouth-theme-breeze kde-config-plymouth colord-kde 
```

After installation edit your grub file:
```
sudo nano /etc/default/grub
```
> Within the quotes on the line that starts with GRUB_CMDLINE_LINUX_DEFAULT, add the option:
```
GRUB_CMDLINE_LINUX_DEFAULT="quiet splash nvidia-drm.modeset=1"
```

Search for #GRUB_GFXMODE, uncomment it and set your screen resolution like 
```
GRUB_GFXMODE=1920x1080
```
> You can also write the color depth:
```
GRUB_GFXMODE=1920x1080x32
```

Save and exit, update grub config:
```
sudo update-grub2
```

Comment your interfaces in /etc/network/interfaces to allow NetworkManager in Plasma control your network connections:

```
sudo nano /etc/network/interfaces
```

Turn everything in this file like:

```
auto wlan0
iface wlan0 inet dhcp
...

# The primary network interface
allow-hotplug enp7s0
iface enp7s0 inet dhcp
```
Into:

```
#auto wlan0
#iface wlan0 inet dhcp
#...
# The primary network interface
#allow-hotplug enp7s0
#iface enp7s0 inet dhcp
```
Reboot system to apply all changes:

```
sudo reboot
```

### Enable file search indexer 

Baloo file indexer is disabled by default in Debian. To enable it go to Search > File Search in system settings or use command

```
sudo balooctl enable
```

### Optionally!
You can install standard or full package of KDE after smallest kde-plasma-desktop install:


```
sudo apt install kde-standard
```

or

```
sudo apt install kde-full
```
>//Delete? On login screen search desktop session button and select Plasma(X11) instead of Plasma(Wayland)

>//Delete? After login into fresh installed desktop you need to install standard sound server on your choice:
**PulseAudio:**
```
sudo apt install pulseaudio pavucontrol pamixer
```
or  **PipeWire**:
https://wiki.debian.org/PipeWire


### Steam install:

```
sudo apt install steam-installer mesa-vulkan-drivers libglx-mesa0:i386 mesa-vulkan-drivers:i386 libgl1-mesa-dri:i386
```

Open "Application Launcher", search for Steam Installer and proceed installation as usual

---

## Pending revision! ### If met screen tearing in DE
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

Install only if going to use Kate
https://apps.kde.org/markdownpart/

https://apps.kde.org/svgpart/


**Apps:**

!OPTIONAL!
https://apps.kde.org/kdenlive/

https://apps.kde.org/krita/


### Sources
https://wiki.debian.org/

https://wiki.debian.org/NvidiaGraphicsDrivers

https://wiki.debian.org/Wayland

https://wiki.debian.org/KDE

https://wiki.debian.org/plymouth

>Delete? https://wiki.debian.org/PulseAudio
https://wiki.debian.org/PipeWire
