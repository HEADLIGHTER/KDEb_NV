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

---

### KDE installation:

Login into your non-root user, install kde-desktop, SDDM, GTK applications theme fixes and SSH if SSH server was installed during Debian installation

```
sudo apt install kde-plasma-desktop sddm breeze-gtk-theme kde-config-gtk-style kde-config-gtk-style-preview ssh
```

Reboot after successful installation of KDE plasma, in lower space of screen press "Desktop session" button and select X11 instead of Wayland

After login into fresh installed desktop you can install standard or full package of KDE:

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

### x86 support:

  
Install x86 support for x86 applications (Steam, Games, Davinci Resolve, etc.)

```
sudo dpkg --add-architecture i386
```

```
sudo apt update
```

```
sudo apt update
```

Nvidia driver for x32 applications. You keep your x64 driver for x64 applications too: 

```
sudo apt install nvidia-driver-libs:i386
```

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

Open "Start Menu" and search for Steam Installer and proceed installation as usual

---

### Enable file search indexer 

Baloo file indexer is disabled by default in Debian. To enable it go to Search > File Search in system settings or execute

```
sudo balooctl enable
```

---

### Optional

Install from Discover:

appstream://org.kde.markdownpart

appstream://org.kde.svgpart

appstream://org.kde.kdenlive.desktop
