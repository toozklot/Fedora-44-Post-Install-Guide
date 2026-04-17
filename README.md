# Fedora 44 Post Install Guide
Things to do after installing Fedora 44

## Set Hostname
* `hostnamectl set-hostname YOUR_HOSTNAME`

## [RPM Fusion remote repository](https://rpmfusion.org/Configuration#Command_Line_Setup_using_rpm)
* Add the RPM Fusion remote repositories and app-stream metadata

## [flathub remote repository](https://flatpak.org/setup/Fedora)
* Add the flathub remote repository

## Update 
* `sudo dnf -y update`
* Reboot

## [Firmware](https://github.com/fwupd/fwupd)
* If your system supports firmware update delivery through lvfs, update your device firmware

## AppImage support

* For AppImage support [install FUSE](https://github.com/AppImage/AppImageKit/wiki/FUSE):
* You can also install an AppImage manager like [Gear Lever](https://flathub.org/apps/it.mijorus.gearlever) for GUI management

## [Media Codecs](https://rpmfusion.org/Configuration)
* Install these to get proper multimedia playback.

## [H/W Video Acceleration](https://fedoraproject.org/wiki/Hardware_Video_Acceleration)
* Helps decrease load on the CPU when watching videos online by alloting the rendering to the dGPU/iGPU. Increases battery life on laptops with iGPUs

## System optimisation
* The tips below can allow you to squeeze out a little bit more performance from your system
* AMD and Nvidea optimisations are not included but can be found [here](https://github.com/winterofhell/fedora-optimizations) (research before running commands)
* Consider installing [ananicy-cpp](https://gitlab.com/ananicy-cpp/ananicy-cpp)

### Remove Gnome Software and manage packages manually
* Gnome software uses  over 100MB of ram at all times for the service it provides, which can be done manually in 30s once per day
* Remove Gnome Software:
* `sudo dnf remove gnome-software` 
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

### Disable ibus-typing-booster
* This service also uses a large amount of ram relative to its usefullness
* Open Gnome Settings → Keyboard (or Region & Language)
* Under Input Sources, click the three-dot menu and select the ibus-typing-booster entry
* Remove it from the list

### Disable unused services
* Skip this step if you need geolocation and cellular services respectively
* `sudo systemctl disable --now geoclue.service ModemManager.service`

### Disable IRQ Balance (Intel iGPU only)
* [Explanation and justification](https://askubuntu.com/questions/1093163/is-irqbalance-a-bad-thing)
```bash
sudo systemctl status irqbalance        # Check status
sudo systemctl disable --now irqbalance # Disable
```

## Steam installation and optimisation
* [Install Steam](https://docs.fedoraproject.org/en-US/gaming/proton/)
* Optionally, [install and enable mangohud](https://docs.fedoraproject.org/en-US/gaming/monitoring/) for benchmarking
* Add launch options to games via Steam → Library → Game → Properties → Launch Options
* Example steam launch options: replace 2560x1440 and 144 with your monitor native resolution and Hz 
`gamescope -f -W 2560 -H 1440 -r 144 --force-grab-cursor --adaptive-sync --hdr-enabled -- %command%`
* Window options: `-f` for fullscreen, `-b` for borderless
* Only include `adaptive-sync` if your dislplay supports FreeSync / G-SYNC
* Only include `hdr-enable` if both your game and display support it
* [More on launch options](https://docs.fedoraproject.org/en-US/gaming/gamescope/)

## Configure Gnome
* Tune Gnome to be more customisable and powerful

### Gnome Extensions
* Install Gnome Extention manager [WIP]
* Essential extentions
```bash
Blur my shell # note: high ram usage
Just perfection
Tiling Shell
User themes
Vitals # requires lm_sensors package
```

### Theming
* In Gnome Settings: choose dark or light mode and set the window border colour
* In Gnome Tweaks: change the defualt wallpaper and system font
* In GDM Settings: customise Gnome Display Manager (login manager)
* Install and configure Rewaita or install a theme from [Gnome Look](gnome-look.org)
* Install a [GRUB theme](https://github.com/jacksaur/Gorgeous-GRUB?tab=readme-ov-file)

## Install Apps
* Compressed files support:
 `sudo dnf install -y unzip p7zip p7zip-plugins unrar`

### Applications to install:
```
Anki (flashcards)
Btrfs Assistant (snapshots)
GoofCord (Discord client)
Drawing (ms paint alternative)
Extension Manager
Flatseal (flatpack permission manager)
Footage (lightweight video editor)
GDM Settings
Gnome Tweaks
Handbrake (video transcoder)
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
* Remove default Gnome apps that aren't useful to you

## Firefox configuration
* Firefox (and its forks) benefit greatly from extentions and other tweaks

### OpenH264 for Firefox
* `sudo dnf install -y openh264 gstreamer1-plugin-openh264 mozilla-openh264`
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
