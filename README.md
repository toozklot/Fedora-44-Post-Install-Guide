# Fedora 44 Post Install Guide
Things to do after installing Fedora 44

## Set Hostname
* `hostnamectl set-hostname YOUR_HOSTNAME`

## [RPM Fusion remote repository](https://rpmfusion.org/Configuration#Command_Line_Setup_using_rpm)
* Add the RPM Fusion remote repositories and app-stream metadata (if using Gnome Software)

## [flathub remote repository](https://flatpak.org/setup/Fedora)
* Add the flathub remote repository

## Update 
* `sudo dnf -y update`
* Reboot

## [Firmware](https://github.com/fwupd/fwupd#basic-usage-flow-command-line)
* If your system supports firmware update delivery through lvfs, update your device firmware

## AppImage support

* For AppImage support [install FUSE](https://github.com/AppImage/AppImageKit/wiki/FUSE):
* You can also install a GUI AppImage manager like [Gear Lever](https://flathub.org/apps/it.mijorus.gearlever)

## [Media Codecs](https://rpmfusion.org/Howto/Multimedia?highlight=%28%5CbCategoryHowto%5Cb%29)
* Install these to get proper multimedia playback.

## [H/W Video Acceleration](https://fedoraproject.org/wiki/Hardware_Video_Acceleration)
* Helps decrease load on the CPU when watching videos online by alloting the rendering to the dGPU/iGPU. Increases battery life on laptops with iGPUs

## System optimisation
* The tips below can allow you to squeeze out a little bit more performance from your system
* Optimisiations for dedicated GPUs not included

### Remove Gnome Software and manage packages manually
* Gnome software uses  over 100MB of ram at all times for the service it provides, which can be done manually in 30s once per day
* Remove Gnome Software and disable packagekit:
```
sudo dnf remove gnome-software
sudo systemctl disable --now packagekit
```
* Manage packages manually:
```bash
sudo dnf install <package>
sudo dnf update -y
sudo dnf remove <package>
sudo dnf autoremove -y # removes orphaned packages
```
* Manage flatpaks manually:
```bash
flatpak install <application>
flatpak update -y
flatpak remove <application>
flatpak uninstall --unused # removes unused runtimes and extentions
```

### Remove Gnome Calendar (optional)
* Gnome Calendar uses tens of MB of ram at idle
* `sudo dnf remove gnome-calendar`
* If the above doesn't work it is installed as a flatpak:
* `flatpak uninstall org.gnome.Calendar `

### Disable unused services
* Skip this step if you need geolocation and cellular services respectively, you can always enable them again if needed
* `sudo systemctl disable --now geoclue.service ModemManager.service`

### Disable IRQ Balance (Intel iGPU only)
* IRQ Balance prevents applications from using 100% of a thread due to sharing with IO tasks
* It also keeps all linux core threads working which is terrible for laptop battery life
```bash
sudo systemctl status irqbalance        # Check status
sudo systemctl disable --now irqbalance # Disable
```

## Steam installation and optimisation
* [Install Steam](https://docs.fedoraproject.org/en-US/gaming/proton/)
* Add launch options to games via Steam → Library → Game → Properties → Launch Options
* Example steam launch options using gamescope:  
  `gamescope -f -W 2560 -H 1440 -r 144 --adaptive-sync --hdr-enabled -- %command%`
* [Fedora docs](https://docs.fedoraproject.org/en-US/gaming/gamescope/)
* [Arch docs](https://wiki.archlinux.org/title/Gamescope#From_Steam)

## Install Apps
* Feel free to use flatpak / rpm as you like

### Applications to install:
```
Anki (flashcards)
btop++
Btrfs Assistant (snapshots)
GoofCord (Discord client)
Extension Manager
Flatseal (flatpack permission manager)
GDM Settings
Gnome Tweaks
Librewolf (privacy focused Firefox fork)
Obsidian (notes)
OpenCloud Desktop sync client (appimage)
Parabolic (video downloader)
Pika Backup (home directory backups)
Rewaita (easy gnome themeing NOTE: uses 60MB ram)
Sound Recorder
Spotify (music streaming)
Upscaler
VS Codium
```

### [Install Tailscale](https://tailscale.com/docs/install/linux)
* For connecting to a NAS / home server

### Remove unused default Gnome apps
* Remove preinstalled Gnome apps that aren't useful to you

## Configure Gnome
* Tune Gnome to be more customisable and powerful

### Gnome Extensions
* Install Gnome Extention manager [WIP]
* Essential extentions
```bash
Blur my shell
Just perfection
Tiling Shell
User themes
Vitals # requires lm_sensors package
```

### Theming
* In Gnome Settings: choose dark or light mode and set the window border colour
* In Gnome Tweaks: change the defualt wallpaper and configure system fonts
* In GDM Settings: customise Gnome Display Manager (login manager)
* [Configure Rewaita](https://github.com/SwordPuffin/Rewaita#-permissions)
* Install a [GRUB theme](https://github.com/jacksaur/Gorgeous-GRUB?tab=readme-ov-file)
* [Install pipes-sh](https://github.com/pipeseroni/pipes.sh#installation)
* Set a profile icon for the user

## Firefox configuration
* Firefox (and its forks) benefit greatly from extentions and other tweaks

### OpenH264 for Firefox
* set `media.gmp-provider.enabled` to true in `about:config`
* Enable the OpenH264 plugin in Firefox settings

### Ram usage
* For systems struggling with ram:
* Lower the value of `dom.ipc.processCount.webIsolated` in `about:config` to 2 or 3

### Firefox extentions
* Essential Firefox extentions
```
Auto Tab Discard (save ram by sleeping inactive tabs)
Bitwarden Password Manager
Canvas Blocker (prevent fingerprinting via javascript)
ClearURLs (automatically remove tracking elements from URLs)
Consent-O-Matic (automatically fill out cookie consent forms)
Obsidian Web Clipper (for Obsidian users)
uBlock Origin (ad blocker)
```

### Theme
* Install Firefox themes in `about:addons`

## Misc
* Stuff that doesen't fit elsewhere

### btop++ configuration
* Pess 'm' to access the menue and change settings e.g. theme
* run `sudo setcap cap_dac_read_search,cap_sys_admin+ep /usr/bin/btop` to give btop++ the necessary permissions to access iGPU stats 
