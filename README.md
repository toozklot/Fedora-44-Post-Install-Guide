# Fedora 44 Post Install Guide
Things to do after installing Fedora 44

## Set Hostname
* `hostnamectl set-hostname YOUR_HOSTNAME`

## [RPM Fusion remote repo](https://rpmfusion.org/Configuration#Command_Line_Setup_using_rpm)
* Add the RPM Fusion remote repositories and app-stream metadata

## [flathub remote repo](https://flatpak.org/setup/Fedora)
* Add the flathub remote repository

## Update 
* `sudo dnf -y update`
* Reboot

## [Firmware](github.com/fwupd/fwupd)
* If your system supports firmware update delivery through lvfs, update your device firmware:
```bash
sudo fwupdmgr get-devices # Lists detected devices
sudo fwupdmgr refresh     # Downloads latest lvfs metadata
sudo fwupdmgr get-updates # Displays available updates
sudo fwupdmgr update      # Downloads and applies updates, or stages them for the next reboot
```

## AppImage

* For AppImage support install fuse:
* `sudo dnf install fuse-libs`
* You can also install an AppImage manager like [Gear Lever](https://flathub.org/apps/it.mijorus.gearlever) for neater management

## [Media Codecs](https://rpmfusion.org/Configuration)
* Install these to get proper multimedia playback.

## [H/W Video Acceleration](https://fedoraproject.org/wiki/Hardware_Video_Acceleration)
* Helps decrease load on the CPU when watching videos online by alloting the rendering to the dGPU/iGPU. Increases battery life on laptops with iGPUs

## System optimisation
* The tips below can allow you to squeeze out a little bit more performance from your system
* AMD and Nvidea optimisations are not included but can be found [here](https://github.com/winterofhell/fedora-optimizations) (research before running commands)
* Consider building ananicy-cpp for more performance (instructions in above link)

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
* Do not do this step if you need geolocation and cellular services respectively
* `sudo systemctl disable --now geoclue.service ModemManager.service`

### Disable IRQ Balance (Intel iGPU only)
* [Explanation and justification](https://askubuntu.com/questions/1093163/is-irqbalance-a-bad-thing)
```bash
sudo systemctl status irqbalance        # Check status
sudo systemctl disable --now irqbalance # Disable
```

## Gaming optimisation

### [Install and configure gamemode](https://github.com/feralinteractive/gamemode)
* gamemode automatically applies gaming optimisations whilst games are running
* Test install: `gamemoded -t`
* Note: you can ignore "ERROR: Governor was not set to performance (was actually powersave)!" as Fedora uses HWP

### Steam optimisation (original guide [here](https://github.com/winterofhell/fedora-optimizations), cherry picked for Intel iGPU)
* Install Steam (flatpak is best) as well as mangohud and gamescope:
```
flatpak install flathub com.valvesoftware.Steam
sudo dnf install mangohud gamescope
flatpak install flathub org.freedesktop.Platform.VulkanLayer.MangoHud
```
* Launch options (paste single line into Steam → Library → Game → Properties → Launch Options). Replace 2560x1440 and 280 with your monitor native resolution and Hz; set fps_limit = Hz − 3.
* Non‑HDR:
`MANGOHUD_CONFIG="fps_limit=277,no_display" mangohud LD_PRELOAD="" PROTON_USE_NTSYNC=1 gamemoderun gamescope -W 2560 -H 1440 -r 280 --force-grab-cursor --adaptive-sync -f -- %command%`
* Auto SDR→HDR:
`MANGOHUD_CONFIG="fps_limit=277,no_display" mangohud LD_PRELOAD="" PROTON_USE_NTSYNC=1 gamemoderun gamescope -W 2560 -H 1440 -r 280 --hdr-enabled --hdr-itm-enabled --hdr-itm-target-nits 1000 --hdr-sdr-content-nits 203 --force-grab-cursor --adaptive-sync --sharpness 2 -f -- %command%`

## Configure Gnome
* Go through all settings in Gnome Settings, Gnome Tweaks, and installed extentions

### Gnome Extensions
* Extend the capabilities of your system
* Make sure that `extention-manager` is installed
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
* Configure Rewaita (see below) or install a theme from [Gnome Look](gnome-look.org)
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
Steam (install the "steam-devices" package for controller support)
Upscaler
VS Codium
```

### [Install Tailscale](https://tailscale.com/docs/install/linux) for an easy, secure connection to a NAS / server

### Remove unused default Gnome apps
* Remove default Gnome apps you wont use

## Firefox configuration
* Firefox (and forks) benefit greatly from extentions and other tweaks


### OpenH264 for Firefox
* `sudo dnf install -y openh264 gstreamer1-plugin-openh264 mozilla-openh264`
* set `media.gmp-provider.enabled` to true in `about:config`
* Enable the OpenH264 plugin in Firefox settings

### Ram usage
* For systems struggling with ram:
* Lower the value of `dom.ipc.processCount.webIsolated` in `about:config` to 2 or 3

### Firefox extentions
```
Auto Tab Discard (save ram by sleeping unused tabs)
Bitwarden Password Manager
Canvas Blocker (prevent fingerprinting via javascript)
ClearURLs (removes tracking elements from URLs)
Consent-O-Matic (automatically fill out cookie consent forms)
Obsidian Web Clipper
uBlock Origin (ad blocker)
```

### Theme
* Install a Firefox theme in `about:addons`
