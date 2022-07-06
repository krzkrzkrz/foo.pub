# Fedora Journey

[Fedora](https://getfedora.org/) creates an innovative, free, and open source platform for hardware, clouds, and containers that enables software developers and community members to build tailored solutions for their users

## Things to do after installing Fedora - Gnome

### Update your system

```shell
sudo dnf update
```

### Enable rpmfusion repos

Enable the Free and Nonfree repositories

```shell
sudo dnf install \
  https://download1.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm

sudo dnf install \
  https://download1.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm
```

Note: The first time you attempt to install packages from these repositories, the dnf utility prompts you to confirm the signature of the repositories. Confirm it

### Gnome tweaks

```shell
sudo dnf install gnome-tweak-tool
```

### Gnome extensions

Go to https://extensions.gnome.org and install the extension on your browser

### TLP

For better laptop battery support

```shell
dnf install tlp tlp-rdw
```

### TLP configuration for Thinkpad laptop’s

The following commands will install Thinkpad-specific packages, which give you more control and information on your Laptop's battery.

```shell
sudo dnf install https://download1.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm
sudo dnf install https://repo.linrunner.de/fedora/tlp/repos/releases/tlp-release.fc$(rpm -E %fedora).noarch.rpm
sudo dnf install kernel-devel akmod-acpi_call akmod-tp_smapi
```

Run the following command to view the battery information and status.

```shell
sudo tlp-stat -b
```

### Better fonts

Enable COPR repository:

```shell
sudo dnf copr enable dawid/better_fonts -y
```

Install packages:

```shell
sudo dnf install fontconfig-font-replacements -y
```

(Optional) Enable subpixel (rgb) antialiasing:

```shell
sudo dnf install fontconfig-enhanced-defaults -y
```

Log out and log in again or restart computer to see the effect

### Install Microsoft fonts (optional)

```shell
sudo dnf install -y curl cabextract xorg-x11-font-utils fontconfig
sudo rpm -i https://downloads.sourceforge.net/project/mscorefonts2/rpms/msttcore-fonts-installer-2.6-1.noarch.rpm
```

### Install Pop fonts (optional)

```shell
sudo dnf install -y fira-code-fonts 'mozilla-fira*' 'google-roboto*'
```

Then go into Gnome Tweaks and make the following changes in Fonts:

```
Interface Text: Fira Sans Book 10
Document Text: Roboto Slab Regular 11
Monospace Text: Fira Mono Regular 11
Legacy Window Titles: Fira Sans SemiBold 10
Hinting: Slight
Antialiasing: Standard (greyscale)
Scaling Factor: 1.00
```

### Install Roboto font

```
sudo dnf install google-roboto-fonts
```

### Google Chrome

Go to the Google Chrome home page, download the `rpm` version and double click to install (preferred way)

- OR -

Enable the repo in the software manager and install it via the software shop

### Slack

Go to the Slack home page, download the `rpm` version and double click to install (preferred way)

- OR -

```shell
sudo dnf -y install wget
wget https://downloads.slack-edge.com/linux_releases/slack-4.14.0-0.1.fc21.x86_64.rpm
sudo dnf localinstall slack-4.14.0-0.1.fc21.x86_64.rpm
```

### Alacritty

```shell
sudo dnf install -y alacritty
```

### VLC

```shell
sudo dnf install -y vlc
```

### Multimedia codecs (optional)

If you have VLC installed, you should be fine as it has builtin support for all relevant audio and video codecs

In other cases, I have found that the following commands install all required stuff for Audio and Video:

```shell
sudo dnf groupupdate sound-and-video
sudo dnf install -y libdvdcss
sudo dnf install -y gstreamer1-plugins-{bad-\*,good-\*,ugly-\*,base} gstreamer1-libav --exclude=gstreamer1-plugins-bad-free-devel ffmpeg gstreamer-ffmpeg 
sudo dnf install -y lame\* --exclude=lame-devel
sudo dnf group upgrade --with-optional Multimedia
```

### Fish shell

```shell
sudo dnf install -y fish util-linux-user
```

Set Fish shell as default

```shell
chsh -s /usr/bin/fish
```

Reboot

### Git

```shell
sudo dnf install -y git
```
