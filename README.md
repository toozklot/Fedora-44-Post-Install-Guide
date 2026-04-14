 # Fedora 44 Post Install Guide
Things to do after installing Fedora 44

## RPM Fusion

* Fedora has disabled the repositories for a lot of free and non-free .rpm packages by default. Follow this if you want to use non-free software like Steam, Discord and some multimedia codecs etc. As a general rule of thumb it is advised to do this to get access to many mainstream useful programs.
* Enable third party repositories by pasting the following into the terminal: 
* `sudo dnf install https://mirrors.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm https://mirrors.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm`
* also while you're at it, install app-stream metadata by:
* `sudo dnf group upgrade core`
  

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

## Flatpak
* Fedora doesn't include all non-free flatpaks by default. The command below enables access to all the flathub flatpaks. Particularly useful for users of Fedora KDE and other spins since they do not get the "Enable Third Party Repositories" option on initial boot.
* `flatpak remote-add --if-not-exists flathub https://dl.flathub.org/repo/flathub.flatpakrepo`

* Fedora doesn't enable Flatpak user-home installation by default, to enable it run:
* `flatpak remote-add --user --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo`

## AppImage

* For Appimage support install fuse:
* `sudo dnf in fuse-libs`
* You can also install an AppImage manager like [Gearlever](https://flathub.org/apps/it.mijorus.gearlever) for neater management. To do so, run the following command:
* `flatpak install it.mijorus.gearlever` 

## Media Codecs
* Install these to get proper multimedia playback.
````
sudo dnf swap 'ffmpeg-free' 'ffmpeg' --allowerasing # Switch to full FFMPEG.
sudo dnf upgrade @multimedia --setopt="install_weak_deps=False" --exclude=PackageKit-gstreamer-plugin # Installs gstreamer components. Required if you use Gnome Videos and other dependent applications.
sudo dnf group install -y sound-and-video # Installs useful Sound and Video complementary packages.
````

## H/W Video Acceleration
* Helps decrease load on the CPU when watching videos online by alloting the rendering to the dGPU/iGPU. Quite helpful in increasing battery backup on laptops.

### H/W Video Decoding with VA-API [look into this]
* `sudo dnf install ffmpeg-libs libva libva-utils`

#### Intel
 
* If you have a recent Intel chipset (5th Gen and above) after installing the packages above., Do:
* `sudo dnf swap libva-intel-media-driver intel-media-driver --allowerasing`
* `sudo dnf install libva-intel-driver`

### OpenH264 for Firefox
* `sudo dnf install -y openh264 gstreamer1-plugin-openh264 mozilla-openh264`
* `sudo dnf config-manager setopt fedora-cisco-openh264.enabled=1`
* set `media.gmp-provider.enabled` to true in about:config
* After this enable the OpenH264 Plugin in Firefox's settings.

## Set Hostname
* `hostnamectl set-hostname YOUR_HOSTNAME`

## Tailscale
* [Installation guide](https://tailscale.com/docs/install/linux)

## Optimisations
* The tips below can allow you to squeeze out a little bit more performance from your system. 

### Disable Gnome Software from Startup Apps [look into this]
* Gnome software autostarts on boot for some reason, even though it is not required on every boot unless you want it to do updates in the background, this takes at least 100MB of RAM upto 900MB (as reported anecdotically). You can stop it from autostarting by:
* `sudo rm /etc/xdg/autostart/org.gnome.Software.desktop`

### Uninstall ram-hungry apps
* Use Gnome Software to uninstall Calendar and Typing Booster

## Gnome Extensions
* Suggestions for good utilities to extend the capabilities of your system
* [User Themes](https://extensions.gnome.org/extension/19/user-themes/)
* [Just Perfection](https://extensions.gnome.org/extension/3843/just-perfection/)
* [Blur My Shell](https://extensions.gnome.org/extension/3193/blur-my-shell/)
* [Clipboard Indicator](https://extensions.gnome.org/extension/779/clipboard-indicator/)
* [Vitals](https://extensions.gnome.org/extension/1460/vitals/)
* [Wireless HID](https://extensions.gnome.org/extension/4228/wireless-hid/)

## Apps
* Packages for Rar and 7z compressed files support:
 `sudo dnf install -y unzip p7zip p7zip-plugins unrar`
* These are Some Packages that I use:
```
Alacritty (performant terminal emulator)
Anki (flashcards)
Btrfs Assistant (snapshots)
Discord
Drawing (ms paint alternative)
Deja Dup Backups 
Extension Manager
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
Upscaler
VS Codium
```
  
## Theming
* Configure Rewaita
* Use an icon pack
* Use a grub theme
* Install a font e.g. Montserrat
