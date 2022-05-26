---
layout: article
title: "Install Nvidia Driver For Ubuntu 16.04 on Sufacebook2"
date: 2020-03-04 11:57:40
Author: Xiang Li
tags: [Ubuntu16.04, Surface, Surface Kernel, DNN]
comments: true
toc: true
---

# Prerequisites

```
sudo apt install git curl wget sed
```
<!--more-->
## Debian Repository

The repository for Debian based distribution (including Ubuntu) can be found at https://pkg.surfacelinux.com/debian/.

Before adding the repository itself, you should import the keys we use to sign the packages.
To do this, run the following two commands:

```
wget -qO - https://raw.githubusercontent.com/linux-surface/linux-surface/master/pkg/keys/surface.asc \
    | sudo apt-key add -
```

After this, you can add the repository itself by running:

```
echo "deb [arch=amd64] https://pkg.surfacelinux.com/debian release main" | sudo tee /etc/apt/sources.list.d/linux-surface.list
```

Then update your local package lists:

```
sudo apt-get update
```

For more information on custom repositories, please have a look
https://wiki.debian.org/SourcesList#Editing_software_sources.

**Install the required packages.**

For Debian:

```
sudo apt-get install linux-headers-surface linux-image-surface linux-libc-dev-surface surface-ipts-firmware linux-surface-secureboot-mok libwacom-surface
```

**Instructions**

0. (Prep) Install Dependencies:

```
 sudo apt install git curl wget sed
```

1. Clone the linux-surface repo:

```
 git clone --depth 1 https://github.com/jakeday/linux-surface.git ~/linux-surface
```

2. Change directory to linux-surface repo:

```
 cd ~/linux-surface
```

3. Run setup script:

```
 sudo sh setup.sh
```

4. Reboot on installed kernel.

The setup script will handle installing the latest kernel for you. You can also choose to download any version you want and install yourself:
Install the headers, kernel and libc-dev (make sure you cd to your download location first):

```
sudo dpkg -i linux-headers-[VERSION].deb linux-image-[VERSION].deb linux-libc-dev-[VERSION].deb
```

## Libwacom (patched)

If you use the Gnome desktop environment, you should install the patched libwacom package.
This allows Gnome to automatically detect your touchscreen and stylus input and rotate it along with the screen.
To check if your device is identified correctly, you can run `libwacom-list-local-devices`.
The patches can be found at https://github.com/linux-surface/libwacom-surface-patches, ready-to-install packages (`libwacom-surface`) in our [[package repositories|Package-Repositories]], or on the Arch user repository (AUR) for Arch Linux users.
Debian packages will also be published at https://github.com/linux-surface/libwacom-surface-deb/releases.

## Surface Control Utility

The [`surface`][surface-control] command line utility (`surface-control` package) aims to provide a unified command line interface to various aspects of Microsoft Surface devices.
At the moment, this is utility is only relevant to the Surface Book 2.
It allows you to control the power state of the discrete GPU (dGPU), change the performance mode (e.g. switch between _Default_, _Battery Saver_, _Better Performance_, and _Best Performance_) impacting the cooling behavior of the device, and control the clipboard latch (e.g. trigger undocking) directly from the command line.
Note that all these aspects can be manually triggered, e.g. via sysfs (see [[1][surface-acpi-perfmode],[2][surface-dgpu]]), and this utility only aims to provide a central, easy to use interface for it.

For more information, please refer to the help functionality of this utility, e.g. by running `surface --help` or `surface <command> --help`.

## Surface DTX Daemon

The [Surface DTX daemon][surface-dtx-daemon] (`surface-dtx-daemon` package) aims to improve and complete the clipboard detachment process on the Surface Books.
Currently, only the Surface Book 2 is supported.
It allows you to take actions in userspace before the clipboard latch opens, such as unmounting any currently mounted USB devices, and lets you abort this process in case of failures.
Additionally, it speeds up the detachment process, as without this tool, the embedded controller will wait until a timeout expires.

In actuality, this package consists of two daemons, the main one run as root and the other run as the currently logged in user(s).
The main daemon can be configured via the [`/etc/surface-dtx/surface-dtx-daemon.conf`](https://github.com/linux-surface/surface-dtx-daemon/blob/master/etc/dtx/surface-dtx-daemon.conf) file and allows you to run scripts when the detachment process starts, when it aborts, and when the tablet has been re-attached.
Please have a look at this file and the [default script](https://github.com/linux-surface/surface-dtx-daemon/blob/master/etc/dtx/detach.sh) run when detachment has been requested for more information on how to configure it.
The per-user daemon takes care of notifications, i.e. it notifies you when the latch has been unlocked and the clipboard can be detached, or when the attachment process has completed and the device can be fully used again.

[surface-control]: https://github.com/linux-surface/surface-control
[surface-acpi-perfmode]: https://github.com/linux-surface/surface-aggregator-module#setting-the-performance-mode
[surface-dgpu]: https://github.com/linux-surface/surface-aggregator-module#controlling-the-dgpu-power-state
[surface-dtx-daemon]: https://github.com/linux-surface/surface-dtx-daemon
[surface-fix-eraser]: https://github.com/linux-surface/surface-fix-eraser

# NVIDIA

[Download](https://www.nvidia.com/Download/index.aspx?lang=en-us)
[Download Archive](https://www.nvidia.com/en-us/drivers/unix/)
[Requirements](http://us.download.nvidia.com/XFree86/Linux-x86_64/384.69/README/minimumrequirements.html)

https://askubuntu.com/questions/832524/possible-missing-frmware-lib-firmware-i915

```
lshw -numeric -C display

lspci | grep -i nvidia
```

The above commands will output your NVIDIA model number.

```
sudo add-apt-repository ppa:graphics-drivers/ppa && sudo apt-get update
#search
sudo apt-cache search nvidia
# or Recommand.
ubuntu-drivers devices
sudo apt install nvidia-390 nvidia-modprobe nvidia-settings nvidia-prime
```

[Install NVIDIA Graphics Driver](https://gist.github.com/wangruohui/df039f0dc434d6486f5d4d098aa52d07#remove-previous-installations--important-)

https://github.com/aimuch/iAI/blob/master/src/ai_base_env.md

**Remove Previous Installations (Important)**

```
sudo apt-get purge nvidia*

# Note this might remove your cuda installation as well
sudo apt-get autoremove
```

As a summary, excuting `sudo apt-get install build-essential gcc-multilib dkms` to install all dependencies.

```
blacklist nouveau
options nouveau modeset=0
```

## modprobe: FATAL: Module nvidia-uvm not found.

----This Part is very important Build if from source is better---

https://blog.csdn.net/yijuan_hw/article/details/53439408

make sure load nvidia driver:`lspci | grep -i nvidia`

`dkms status` :bbswitch, 0.8: added nvidia, 430.64: added

```
dkms build -m nvdia -v 430.64
/var/lib/dkms/nvidia/430.64/source
```

---

## CUDA

[cuda-toolkit-archive](https://developer.nvidia.com/cuda-toolkit-archive)

```
# 设置路径
vi ~/.bashrc

export CUDA_HOME=/usr/local/cuda-10.1
export PATH=${CUDA_HOME}/bin:$PATH
export LD_LIBRARY_PATH=${CUDA_HOME}/lib64/:$LD_LIBRARY_PATH

source ~/.bashrc
```

```
# 查看cuda版本，测试是否安装成功
cat /usr/local/cuda/version.txt
nvcc -V

# 编译 samples
cd /usr/local/cuda-10.0/samples
sudo make
cd /usr/local/cuda/samples/bin/x86_64/linux/release
./deviceQuery
```

## Delte Kernel

Install synaptic

```
sudo apt install synaptic
```

Find all kernel by _linux-image,linux-header_
update grub2 `sudo update-grub2`
List all kernels

```
dpkg --get-selections | grep linux
sudo apt-get remove linux-image-
```

## SSH Key

```
ls -al ~/.ssh
# Lists the files in your .ssh directory, if they exist
ssh-keygen
```

create confing 

```
cd ~/.ssh/
touch config
```

add following to config.

```
# github
  Host github.com
  HostName github.com
  PreferredAuthentications publickey
  IdentityFile ~/.ssh/id_rsa_github_linux
```

# [Terminator](https://github.com/EliverLara/terminator-themes)

**1. Install the [requests](https://github.com/requests/requests) python package**

```sh
 pip install requests
```
> **Note:** If you're using python 3, ignore the previous command and install the `python2-requests` package for your specific OS.

**2. Create plugins directory if it does not exist**

```sh
 mkdir -p $HOME/.config/terminator/plugins
```

**3. Getting the correct plugin**

For terminator >= 1.9

```sh
 wget https://git.io/v5Zww -O $HOME"/.config/terminator/plugins/terminator-themes.py"
```

For terminator < 1.9

```sh
 wget https://git.io/v5Zwz -O $HOME"/.config/terminator/plugins/terminator-themes.py"
```

**4. Activation**

Check the `TerminatorThemes` option under `terminator > preferences > plugins`.

# Jekyll
1.安装ruby

```
$ sudo apt-add-repository ppa:brightbox/ruby-ng
$ sudo apt-get update
```
`sudo apt install ruby-full ruby-bundler`

https://blog.walterlv.com/post/setup-jekyll-in-windows.html

2.安装jekyll

```
sudo gem install jekyll
sudo apt-get install ruby2.6 ruby2.6-dev
```
3.检测jekyll是否安装成功

```

jekyll -v
gem install jekyll-sitemap
gem install jekyll-feed
gem install jekyll-paginate

jekyll serve
```
# Fcitx
sudo apt install fcitx 
2，fcitx不自启，这该怎么办呀？

GUI界面：

system settings > startup and shutdown > auto-start> add program


3，fcitx无法在所用应用中使用或是启动后没办法使用

fcitx-diagnose
终端中输入上面的代码，你只需要注意红字部分就可以了，会自动给出解决方案的。

一般来说都是fcitx桌面文件未配置好，这种情况尤其容易出现在最新的Gnome和Kde上，因为二者已经迁移到了wayland，而fcitx的配置文件还停留在以前的xorg上。

通过代码，列出问题，如果是让你编辑.xprofile文件，请直接忽略，你可以直接复制代码了。

GTK_IM_MODULE=fcitx
QT_IM_MODULE=fcitx
XMODIFIERS=@im=fcitx
将上述代码粘贴进/etc/enviroment中即可，别忘了用sudo，etc是根目录下的文件夹哦！

至此，你的fcitx应该就是配置完成了，注销一下就可以用了。

# Boot with gpu on

To your actual question: The default power-state is controlled via module parameters. There's some info about this on the SAM repo: 
https://github.com/linux-surface/surface-aggregator-module#controlling-the-dgpu-power-state. 

In essence:

Create a new .conf file in ```/etc/modprobe.d/, e.g. /etc/modprobe.d/dgpu.conf```
In the file, add the line ```options surface_sam_hps dgpu_power_init=1 ```(if you use the SAM module via the kernel, or ```options surface_sam dgpu_power_init=1``` if you've manually installed the SAM module from the repo).
Reboot.

Of course you could also write a systemd service to just run surface dgpu set on (or any kind of script that checks that based on some condition) before the graphical target if you want to have more control.

# Delete Kernel

https://www.zybuluo.com/breakerthb/note/787656

# Reference

- [ubuntu 16.04](https://turlucode.com/how-to-install-ubuntu-16-04-on-surface-book-2/)
- [ubuntu 18.04](https://turlucode.com/how-to-install-ubuntu-18-04-on-surface-book-2/)
- [jakeday linux-surface](https://github.com/jakeday/linux-surface)
- [Linux-Surface Kernel](https://github.com/linux-surface)
- [Utilities and Packages](https://github.com/linux-surface/linux-surface/wiki/Utilities-and-Packages)
- [Linux Surface Kernel Installation](https://github.com/linux-surface/linux-surface/wiki/Installation-and-Setup)
- [Terminator](https://github.com/EliverLara/terminator-themes)

