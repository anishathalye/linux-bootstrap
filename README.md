# linux bootstrap

This probably isn't useful to anyone, and I'm not really maintaining it as an
open-source project. But I thought I'd keep it public in case anyone wants to
see...

Tested on Debian Jessie and Debian Stretch.

## install and configure sudo

```
su
apt install sudo
usermod -aG sudo anish
```

## enable non-free components

Edit `/etc/apt/sources.list` and add `non-free` to the end of all the lines.

## machine-specific

### ithilien

```
sudo apt install firmware-realtek
```

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
    git mercurial vim htop iotop axel aria2 silversearcher-ag \
    build-essential libevent-dev libncurses-dev \
    autojump python-pip python-virtualenv python-dev \
    vnstat apt-transport-https lm-sensors bc \
    openjdk-8-jdk rsync
```

### scientific computing

```
sudo apt install python-numpy python-scipy \
    gfortran libblas-dev liblapack-dev \
    libjpeg-dev zlib1g-dev
```

## build programs from source

```
mkdir -p ~/downloads

cd ~/downloads
wget 'https://github.com/tmux/tmux/releases/download/2.5/tmux-2.5.tar.gz'
tar xvf tmux-2.5.tar.gz
cd tmux-2.5
./configure
make -j12
sudo make install

cd ~/downloads
wget 'https://downloads.sourceforge.net/project/zsh/zsh/5.4.1/zsh-5.4.1.tar.xz'
tar xvf zsh-5.4.1.tar.xz
cd zsh-5.4.1
./configure
make -j12
sudo make install
sudo sh -c 'echo "/usr/local/bin/zsh" >> /etc/shells'
chsh -s /usr/local/bin/zsh
```

## set up a ssh key

```
ssh-keygen -t rsa -b 4096
```

Add `~/.ssh/id_rsa.pub` to GitHub account.

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
git checkout gondor # has path setup for cuda stuff
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
