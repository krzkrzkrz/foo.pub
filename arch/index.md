# Arch Journey

[Arch](https://archlinux.org), a simple light-weight Linux distribution

[Installation notes](http://foo.pub/arch/install)

## Update mirrors and packages

```shell
sudo pacman -Syyyu
```

## Install Spectrwm

```shell
sudo pacman -S xorg xorg-xinit xf86-video-intel spectrwm dmenu xterm
```

Copy default spectrwm conf

```shell
cp /etc/spectrwm.conf .spectrwm.conf
```

Modify `vim .xinitrc` with

```shell
exec spectrwm
```

To run `Spectrwm`

```shell
startx
```

In Spectrwm, to open a terminal, type: `mod+shift+enter`

## Dotfile management

```shell
cd ~
git init
echo "/*" > .gitignore
git add -f .gitignore
git commit -m "add .gitignore"
git add -f .profile
git commit -m "add .profile"
```

You can also add folders you want to not be ignored to `.gitignore` with `!` at the start like this

```
!/.config
!/.config/fish
/.config/fish/fish_history
/.config/fish/fish_variables
/.config/fish/fishd.*
```

When doing a fresh install, just do `git clone https://github.com/<username>/dotfiles`

Which clones the repo into a new folder called `dotfiles`, and then move everything out of `dotfiles` into `~` (including the `.git` folder), overwriting any existing files.

## Sudo

[Sudo](https://wiki.archlinux.org/index.php/Sudo)

Allow users to execute the `sudo` command. i.e. `pacman -S vim`

```shell
sudo pacman -S sudo
```

```shell
vim etc/sudoers
```

To allow all users to execute the `sudo` command. Make sure the following is uncommented:

```
wheel ALL=(ALL) ALL
```

## AUR helper

Use an AUR helper to instal AUR packages easier

```shell
git clone https://aur.archlinux.org/yay.git
cd yay/
makepkg -sri
```

Install a package:

```shell
yay -S ttf-iosevka
```

Search for a package name

```shell
yay -Ss ttf-iosevka
```

Get more information about a package

```shell
yay -Si ttf-iosevka
```

Update all packages AUR and offical

```shell
yay -Syu
```

List all packages that require an update

```shell
yay -Pu
```

Cleaning unwanted dependencies

```shell
yay -Yc
```

To remove / uninstall package

```shell
yay -R ttf-iosevka
```

To remove / uninstall package and dependencies

```shell
yay -Rs ttf-iosevka
```

## NTP

Network Time Protocol daemon

```shell
sudo pacman -S ntp
systemctl enable ntpd.service
systemctl status ntpd.service
```

## Audio / Microphone

```shell
pacman -S alsa-utils
pacman -S pulseaudio
pacman -S pulseaudio-alsa
```

You can install `pavuctrol` for a graphical UI to manage devices

```shell
pacman -S pavucontrol
```

## Power saving

```shell
pacman -S tlp
```

## Setting hibernation

Things to know:

**Suspend**, saving the state to RAM
Meaning, system does not completely shut-off. Machine still supplies power to the RAM. Downside to this, if system runs out of power, data is lost

**Hibernate**, saving the state to disk.
System shuts down, power is saved. Data is also retained on reboot.

**Sleep**, Is a hybrid of suspend and hibernate.
Save the system state to RAM and saving user space to disk. Allows the system to remain on, so you can quickly resume, but its using less power because user space, all currently opened programs is saved to disk

### 1) Make sure the swapfile is big enough

For 32GB ram, we set swap file to 1.5x. So to 48gb, which is 48000MB

If existing swap file exists. Remove it first:

```shell
swapoff /swapfile
rm -f /swapfile
```

Then create a new swapfile:

```shell
dd if=/dev/zero of=/swapfile bs=1M count=512 status=progress
chmod 600 /swapfile
mkswap /swapfile
swapon /swapfile
```

Make sure, at `/etc/fstab`, the following is still there:

```shell
/swapfile none swap defaults 0 0
```

### 2) Get the `UUID`

For a standard root partition we need the partition that is mounted to root (/) and it's `UUID`.

You can do this with runnint `df` in a terminal. We're looking for `Mounted On` and `/`. In the example below, we can see that `/dev/nvme0n1p2` is mounted on `/`. For example:

```shell
$ df
Filesystem     1K-blocks     Used Available Use% Mounted on
dev             16239140        0  16239140   0% /dev
run             16248284     1332  16246952   1% /run
/dev/nvme0n1p2 244580992 80849208 151238100  35% /
```

To get the UUID, type `sudo blkid`:

```shell
$ sudo blkid
/dev/nvme0n1p1: UUID="E073-485C" BLOCK_SIZE="512" TYPE="vfat" PARTUUID="48229116-98ef-9745-bd3e-339c76239c93"
/dev/nvme0n1p2: UUID="88672022-6816-428f-9ff8-38329a85cde5" BLOCK_SIZE="4096" TYPE="ext4" PARTUUID="67d1f955-4e56-ea41-927c-25a285e513a2"
```

`88672022-6816-428f-9ff8-38329a85cde5` is the `UUID`.

### 3) Get the `physical offset of /swapfile`

```shell
$ sudo filefrag -v /swapfile
Filesystem type is: ef53
File size of /swapfile is 50331648000 (12288000 blocks of 4096 bytes)
 ext:     logical_offset:        physical_offset: length:   expected: flags:
   0:        0..  425983:    2719744..   3145727: 425984:
   1:   425984..  456703:    5703680..   5734399:  30720:    3145728:
   2:   456704..  485375:    5738496..   5767167:  28672:    5734400:
   3:   485376..  489471:    5812224..   5816319:   4096:    5767168:
   ...
```

The value we are after is the `ext 0`, `physical offset` (first value of physical_offset).

Which is `2719744`

### 4) Update grub config to resume (hibernate) on boot

With the `UUID` and `physical offset` values known. We can now edit GRUB:

```shell
sudo vim /etc/default/grub
```

Update the `GRUB_CMDLINE_LINUX_DEFAULT` value to include `resume` and `resume_offset`. For example:

```shell
GRUB_CMDLINE_LINUX_DEFAULT="loglevel=3 quiet resume=UUID=88672022-6816-428f-9ff8-38329a85cde5 resume_offset=2719744"
```

Then generate the new GRUB config file:

```shell
sudo grub-mkconfig -o /boot/grub/grub.cfg
```

### 5) Add `resume` to `mkinitcpio HOOKS`

```shell
sudo vim /etc/mkinitcpio.conf
```

Look for `HOOKS` and add `resume`, for example:

```shell
HOOKS=(base udev autodetect modconf block resume filesystems keyboard fsck)
```

Generate the images:

```shell
sudo mkinitcpio -p linux
```

To test if hibernate works, type `systemctl hibernate`.

Wait few seconds, screen may flicker. System will shutdown. Press power-on button. GRUB should show up. At this point. Dont need to sign in anymore, system should take you straight to last session.

### Update `HandleLidSwitch` for more power saving

Set the `HandleLidSwitch` from `suspend` to `hybrid-sleep`, to save more power

```shell
sudo vim /etc/systemd/logind.conf
```

After setting above, run `sudo systemctl restart systemd-logind` (system will reboot)

### Hibernate on low battery level

```shell
sudo vim /etc/udev/rules.d/99-lowbat.rules
```

Make sure the following is there:

```shell
# Suspend the system when battery level drops to 5% or lower
SUBSYSTEM=="power_supply", ATTR{status}=="Discharging", ATTR{capacity}=="[0-5]", RUN+="/usr/bin/systemctl hibernate"
```

## Packages

```shell
pacman -S fish
pacman -S base-devel
pacman -S unzip
pacman -S kitty
pacman -S rofi
pacman -S tmux
pacman -S maim
pacman -S ntp
yay -S rofi-keepassxc
rofi-keepassxc
yay -S betterlockscreen
pacman -S ttf-roboto
yay -S ttf-iosevka
yay -S nerd-fonts-complete
yay -S nerd-fonts-complete
pacman -S picom
pacman -S xclip
yay -S alttab
pacman -S rg
pacman -S bat
pacamn -S fzf
pacamn -S autocutsel
pacamn -S unclutter
pacamn -S dunst
pacamn -S papirus-icon-theme
pacman -S perl-pod-parser
yay -S tdrop-git
pacman -S redis
pacman -S exa
pacman -S discord
```

## Install and configure rbenv

Before compiling, make sure the following dependencies are installed first

```shell
pacman -S --needed base-devel libffi libyaml openssl zlib
```

```shell
yay -S rbenv
yay -S ruby-build
```

Optionally, you can go ahead and install a Ruby version

```shell
rbenv install 2.3.6
rbenv global 2.3.6
```

Note, when install Ruby version `2.3.6`, since this is a much older version of Ruby. You need to install the following package first: `openssl-1.0`

And then do:

```shell
PKG_CONFIG_PATH=/usr/lib/openssl-1.0/pkgconfig rbenv install 2.3.6
```

## Configure Fish shell

To list shells

```shell
chsh -l
```

Set fish as default shell

```shell
chsh -s /usr/bin/fish
```

## Configure Vim - Dracula theme

In `.vim/plugged/dracula/colors/dracula.vim`, (usually at line 236), comment:

```shell
hi! link TabLineSel Normal
```

In `.vim/plugged/dracula/autoload/lightline/colorscheme/dracula.vim`, 

1) At around line 19, add:

```shell
let s:none = ['NONE', 'NONE']
```

2) Usually at line 32, edit from:

```shell
let s:p.normal.middle = [ [ s:white, s:gray ]]
```

To:

```shell
let s:p.normal.middle = [ [ s:white, s:none ]]
```

3) Usually at line 35, edit from:

```shell
let s:p.inactive.middle = [ [ s:white, s:gray ] ]
```

To:

```shell
let s:p.inactive.middle = [ [ s:white, s:none ] ]
```

## Font scaling

Instead of modifying X's dpi settings. Just modify the font sizes for each application

## Setup .xinitrc

TODO

## Reconfiguring keys

TODO

## Fix paste

TODO

## Install Vim

```shell
sudo pacman -S vim
```

### Python is needed for some Vim plugns to work properly:

```shell
sudo pacman -S python
```

### Setup Keepassxc

```shell
pacman -S keepassxc
```

By default and depending on the screen resolution. Keepassxc fonts and icons may look small.

To fix this, add an alias at `.config/fish/aliases.fish`:

```shell
alias keepass 'QT_FONT_DPI=192 keepassxc'
```

And to run Keepassxc, just do `keepass` instead of `keepassxc`

```shell
QT_FONT_DPI=192
```

Make sure you restart for changes to take effect.

### Configure Vim with a package manager (Plug):

```shell
curl -fLo ~/.vim/autoload/plug.vim --create-dirs \
    https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
```

### Install xmonad

TODO

### Install FZF

```shell
sudo pacman -S fzf
```









```shell
```

