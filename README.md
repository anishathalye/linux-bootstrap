# linux bootstrap

This probably isn't useful to anyone, and I'm not really maintaining it as an
open-source project. But I thought I'd keep it public in case anyone wants to
see...

Tested on Debian Stretch.

## install and configure sudo

```
su
apt install sudo
usermod -aG sudo anish
```

## enable non-free components

Edit `/etc/apt/sources.list` and add `non-free` to the end of all the lines.

## machine-specific

### bongo (AMD A8-3850)

```
sudo apt install firmware-realtek
```

### mordor (r710)

```
sudo apt install firmware-bnx2 firmware-qlogic
```

**This firmware (bnx2) is required to use the NICs at all, so it's needed
during install to configure the network. Using the Debian distribution that
includes non-free firmware solves this problem (because it includes bnx2).**

## set up dynamic dns

Look at "quick cron example".

## ssh server

Add ssh key to `~/.ssh/authorized_keys`.

Disable SSH password login in `/etc/ssh/sshd_config` by setting
`PasswordAuthentication no`.

## install programs

```
sudo apt update
sudo apt install \
    curl git mercurial vim htop iotop axel aria2 silversearcher-ag \
    build-essential libevent-dev libncurses-dev \
    autojump python-pip python-virtualenv python-dev \
    vnstat lm-sensors bc rsync \
    tmux zsh
```

### scientific computing

```
sudo apt install python-numpy python-scipy \
    gfortran libblas-dev liblapack-dev \
    libjpeg-dev zlib1g-dev python-opencv
```

## set default shell

```
chsh -s /usr/bin/zsh
```

## set up a ssh key

```
ssh-keygen -t ed25519 -a 100
```

Note that the `-a 100` is the number of rounds of the KDF to use, so it doesn't
matter for an unencrypted key.

Add `~/.ssh/id_ed25519.pub` to GitHub account.

## install dotfiles

```
mkdir -p ~/src
cd ~/src
git clone git@github.com:anishathalye/dotfiles
cd dotfiles
./install
cd ~/src
git clone git@github.com:anishathalye/dotfiles-local
cd dotfiles-local
git checkout linux-server
./install
```

## gpu setup

```
cd ~/src/dotfiles-local
git checkout gpu
./install
```

* [NVIDIA drivers](http://www.nvidia.com/object/unix.html)
* [CUDA toolkit](https://developer.nvidia.com/cuda-downloads)
    * Get kernel headers first: `sudo apt install linux-headers-$(uname -r)`
    * Using the Ubuntu `.run` file should work
    * Reboot afterwards
* [cuDNN](https://developer.nvidia.com/rdp/cudnn-download)
    * Accelerated Computing Developer Program membership required
    * Copy files to `/usr/local/cuda`
