# Fedora 44 Post Install Guide
Things to do after installing Fedora 44

## [RPM Fusion & app-stream metadata](https://rpmfusion.org/Configuration#Command_Line_Setup_using_rpm)
* Add the RPM Fusion repositories and app-stream metadata

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

## [Flathub](https://flathub.org/en/setup/Fedora)

* Fedora doesn't enable flatpak user-home installation by default, enable it:
* `flatpak remote-add --user --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo`

## AppImage

* For AppImage support install fuse:
* `sudo dnf install fuse-libs`
* You can also install an AppImage manager like [Gearlever](https://flathub.org/apps/it.mijorus.gearlever) for neater management

## [Media Codecs](https://rpmfusion.org/Configuration)
* Install these to get proper multimedia playback.

## [H/W Video Acceleration](https://fedoraproject.org/wiki/Hardware_Video_Acceleration)
* Helps decrease load on the CPU when watching videos online by alloting the rendering to the dGPU/iGPU. Increases battery life on laptops with iGPUs

### OpenH264 for Firefox
* `sudo dnf install -y openh264 gstreamer1-plugin-openh264 mozilla-openh264`
* set `media.gmp-provider.enabled` to true in `about:config`
* After this enable the OpenH264 Plugin in Firefox's settings.

## Set Hostname
* `hostnamectl set-hostname YOUR_HOSTNAME`

## [Tailscale](https://tailscale.com/docs/install/linux)
* Install Tailscale for an easy, secure connection to your NAS / server

## Optimisations
* The tips below can allow you to squeeze out a little bit more performance from your system
* AMD and Nvidea optimisations are not included but can be found [here](https://github.com/winterofhell/fedora-optimizations) (research before running commands)

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
* Open Gnome Settings -> Keyboard (or Region & Language)
* Under Input Sources, click the three-dot menu and select the ibus-typing-booster entry
* Remove it from the list

### Disable service for mobile broadband devices & USB cellular modems
* `sudo systemctl disable --now ModemManager.service`

### [Install and configure gamemode](https://github.com/feralinteractive/gamemode)
* gamemode automatically applies gaming optimisations whilst games are running

### Disable IRQ Balance (Intel iGPU only)
* [Explanation and justification](https://askubuntu.com/questions/1093163/is-irqbalance-a-bad-thing)
```bash
sudo systemctl status irqbalance        # Check status
sudo systemctl disable --now irqbalance # Disable
```

## Gnome Extensions
* Extend the capabilities of your system
```bash
Blur my shell
Just perfection
User themes
Vitals # requires lm_sensors package
```
## Configure GNOME
* Go through all settings in Gnome Settings, Gnome Tweaks, and installed extentions

## Apps
* Compressed files support:
 `sudo dnf install -y unzip p7zip p7zip-plugins unrar`
* These are Some Packages that I use:
```
Anki (flashcards)
Btrfs Assistant (snapshots)
Discord
Drawing (ms paint alternative)
Extension Manager
Flatseal (flatpack permission manager)
Footage (lightweight video editor)
GDM Settings
Gnome Tweaks
Handbrake (video transcoder)
Khronos (timers)
Librewolf
lm_sensors (required for vitals extention)
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
  
## Theming
* Choose dark or light mode and set the window border colour in Gnome Settings
* Set a background in Gnome Tweaks
* Configure Rewaita or install a theme from [Gnome Look](gnome-look.org)
* Install a [GRUB theme](https://github.com/jacksaur/Gorgeous-GRUB?tab=readme-ov-file)
* Change the system font using Gnome Tweaks
* Theme GDM using GDM Settings

## Firefox 
* Firefox (and forks) benefit greatly from extentions and other tweaks

### Ram usage
* For systems with <= 8GB ram:
* Lower the value of `dom.ipc.processCount.webIsolated` in `about:config` to 2 or 3

### Extentions
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
* Install a Firefox theme in about:addons

## Configure GNOME
* Go through all settings in Gnome Settings, Gnome Tweaks, and Just Perfection

## [Performance optimisation guide](https://github.com/winterofhell/fedora-optimizations) [go through this and cherry pick]
* WIP copying over stable stuff
* Sections remaining to look into:
```
I/O Scheduler Configuration
Steam optimisations
```
