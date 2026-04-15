# Fedora 44 Post Install Guide
Things to do after installing Fedora 44

## [RPM Fusion & app-stream metadata](https://rpmfusion.org/Configuration#Command_Line_Setup_using_rpm)
* Add the RPM Fusion repositories and app-stream metadata

## Update 
* `sudo dnf -y update`
* Reboot

## Firmware
* If your system supports firmware update delivery through lvfs, update your device firmware by:
```
sudo fwupdmgr refresh --force
sudo fwupdmgr get-devices # Lists devices with available updates.
sudo fwupdmgr get-updates # Fetches list of available updates.
sudo fwupdmgr update
```

## [Flathub](https://flathub.org/en/setup/Fedora)

* Fedora doesn't enable Flatpak user-home installation by default, to enable it run:
* `flatpak remote-add --user --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo`

## AppImage

* For Appimage support install fuse:
* `sudo dnf install fuse-libs`
* You can also install an AppImage manager like [Gearlever](https://flathub.org/apps/it.mijorus.gearlever) for neater management

## [Media Codecs](https://rpmfusion.org/Configuration)
* Install these to get proper multimedia playback.

## [H/W Video Acceleration](https://fedoraproject.org/wiki/Hardware_Video_Acceleration)
* Helps decrease load on the CPU when watching videos online by alloting the rendering to the dGPU/iGPU. Quite helpful in increasing battery backup on laptops.

### OpenH264 for Firefox
* `sudo dnf install -y openh264 gstreamer1-plugin-openh264 mozilla-openh264`
* `sudo dnf config-manager setopt fedora-cisco-openh264.enabled=1`
* set `media.gmp-provider.enabled` to true in about:config
* After this enable the OpenH264 Plugin in Firefox's settings.

## Set Hostname
* `hostnamectl set-hostname YOUR_HOSTNAME`

## [Tailscale](https://tailscale.com/docs/install/linux)
* Install Tailscale for an easy, secure connection to your NAS / server

## Optimisations
* The tips below can allow you to squeeze out a little bit more performance from your system

### Disable Gnome Software from Startup Apps [look into this]
* Gnome software autostarts on boot for some reason, even though it is not required on every boot unless you want it to do updates in the background, this takes at least 100MB of RAM upto 900MB (as reported anecdotically). You can stop it from autostarting by:
* `sudo rm /etc/xdg/autostart/org.gnome.Software.desktop`

### Uninstall ram-hungry apps
* Use Gnome Software to uninstall Calendar and Typing Booster

## Gnome Extensions
* Good utilities to extend the capabilities of your system (except BMS, which eats ram and just looks cool)
* [User Themes](https://extensions.gnome.org/extension/19/user-themes/) (for theming gnome shell)
* [Just Perfection](https://extensions.gnome.org/extension/3843/just-perfection/)
* [Blur My Shell](https://extensions.gnome.org/extension/3193/blur-my-shell/)
* [Vitals](https://extensions.gnome.org/extension/1460/vitals/)

## Apps
* Packages for Rar and 7z compressed files support:
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
Pika backup (modern alternative to Deja Dup Backups)
Rewaita (easy gnome themeing)
Sound Recorder
Spotify (music streaming)
Steam (install the "steam-devices" package for controller support)
Upscaler
VS Codium
```
  
## Theming
* Configure Rewaita
* Install a [GRUB theme](https://github.com/jacksaur/Gorgeous-GRUB?tab=readme-ov-file)
* Install and use a sytem wide font using GNOME Tweaks
* Set a background in GNOME Tweaks
* Use GDM Settings to theme GDM (the default is hideous)

## Firefox 
* Firefox by default isn't the best experience

### Extentions
* uBlock Origin (add blocker)
* New Tab Override (for use with self hosted homepage)
* Auto Tab Discard (save ram by sleeping unused tabs)
* Consent-O-Matic (automatically fill out cookie consent forms)
* Canvas Blocker (prevent fingerprinting via javascript)
* Bitwarden Password Manager
* ClearURLs
* Clear Cache
* Obsidian Web Clipper
* Wayback Machine

### Theme
* Install a theme

## Configure GNOME
* Simply run through all settings in GNOME Settings, GNOME Tweaks, and Just Perfection

## [Performance optimisation guide](https://github.com/winterofhell/fedora-optimizations)
* Most of the above guide likely isn't worth the hassle, but it's a good idea to look through it
