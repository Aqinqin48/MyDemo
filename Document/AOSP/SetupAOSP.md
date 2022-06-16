# 1. Install WSL

If you have installed the wsl version **1**, update it to 2 by: 

wsl --set-version Ubuntu 2

How long it takes depends on how big your wsl is.

If everything is good, go to chapter 2.

 

**Make sure you C drive has more than 300GB room, or find a way to install WSL on another drive.**

First try wsl --install. If everything is good, **CONGRATULATIONS**this step is done.

If not, 

**follow the following 6 steps strictly.**

- **1. Enable the Windows Subsystem for Linux**

​		Open PowerShell as Administrator and run:

​		dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart

 

- **2. Check requirements for running WSL 2**

​		Open **winver**, and make sure **Version 1903**or higher, with **Build 18362**or higher, otherwise try to update your Windows.

 

- **3. Enable Virtual Machine feature**

​		Open PowerShell as Administrator and run:

​		dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart

​		**Restart**your machine.

 

- **4. Download the Linux kernel update package**

​		Download and run the following:

[		WSL2 Linux kernel update package for x64 machines](https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi)

 

- **5. Set WSL 2 as your default version**

​		wsl --set-default-version 2

- **6. Install your Ubuntu**

​		[Ubuntu 20.04 LTS](https://www.microsoft.com/store/apps/9n6svws3rx71)

​		Install it and run it.

 

- **7. (Optional) Install zsh, oh-my-zsh and a theme**

​		sudo apt install zsh

​		sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"

​		git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k

​		Set ZSH_THEME="powerlevel10k/powerlevel10k" in ~/.zshrc.

 

- **8. (Optional) Install Windows Terminal**

​		Windows Terminal enables multiple tabs (quickly switch between multiple Linux command lines, Windows Command Prompt, PowerShell, Azure CLI, etc), create custom key bindings (shortcut keys for opening or closing tabs, copy+paste, etc.), use the search feature, and custom themes (color schemes, font styles and sizes, background image/blur/transparency).

​		It can be easily installed via Store.

#  2. Download AOSP repo

- **1. Install repo and gain credentials to all git repositories**

​		sudo wget ' [https://storage.googleapis.com/git-repo-downloads/repo'](https://storage.googleapis.com/git-repo-downloads/repo) -P /usr/local/sbin/

​		sudo chmod a+x /usr/local/sbin/repo

- **2. Set git identities**

​		Get prepared for repo init. 

​		git config --global user.email mmx@microsoft.com

​		git config --global user.name mmx

- **3. Initialize the AOSP repository source code**

​		I recommend using the home directory instead of /mnt/\*, because some problems about filesystem may occur. 

​		mkdir aosp && cd aosp

​		Init the repo **without**sudo

​		repo init -u https://android.googlesource.com/platform/manifest -b android-12.0.0_r3

​		If there are some issues, please run below command and try again:

​		sudo ln -s /usr/bin/python3 /usr/bin/python

 

- **4. Sync code**

​		repo sync

​		This may take a long time.

​		You can continue on the 3rd or 4th chapter, but remember to get back here.

- **5. Download drivers**

​		找到对应 Drivers下载地址

​		https://source.android.google.cn/setup/start/build-numbers?hl=zh-cn

​		https://developers.google.com/android/drivers#crosshatchsp1a.210812.016.a1

​		wget these files into the **root of the AOSP directory**to get RQ2A.210405.005 (flame for Pixel 4,coral for Pixel 4XL **)**

​	**Pixel 3XL****驱动**

​		wget https://dl.google.com/dl/android/aosp/google_devices-crosshatch-sp1a.210812.016.a1-63881ed7.tgz
​		wget https://dl.google.com/dl/android/aosp/qcom-crosshatch-sp1a.210812.016.a1-246dd7c9.tgz

 

​	**Pixel 5****驱动**

​		**wget** [**https://dl.google.com/dl/android/aosp/google_devices-redfin-sp1a.210812.016.a1-8813b219.tgz**](https://dl.google.com/dl/android/aosp/google_devices-redfin-sp1a.210812.016.a1-8813b219.tgz)

​		wget https://dl.google.com/dl/android/aosp/qcom-redfin-sp1a.210812.016.a1-8d32b5b1.tgzmkdir

​	Once these are downloaded, unzip them and run the scripts.

​		tar -xzf qcom* 

​		tar -xzf google_devices* 

​		bash extract-google_devices-coral.sh 

​		bash extract-qcom-coral.sh

​		bash extract-qcom-crosshatch.sh 

​		bash extract-google_devices-crosshatch.sh

 

- **6. Install some necessary packages**

​		sudo apt-get install git-core gnupg flex bison build-essential zip curl zlib1g-dev gcc-multilib g++-multilib libc6-dev-i386 lib32ncurses5-dev x11proto-core-dev libx11-dev lib32z1-dev libgl1-mesa-dev libxml2-utils xsltproc unzip fontconfig

​		sudo apt install libncurses5

 

​	**Switch to the OEM branch****（****framework/****base****）：**

​		git init

​		git remote add origin https://microsoft@dev.azure.com/microsoft/MMX.OEM/_git/platform.frameworks.base

​		git fetch

​		git checkout oem/development_12

 

- **7. Build for the first time**

​		source build/envsetup.sh

​		lunch 

​		m

​		This may take a long time.

​		You can continue to the next chapter while it is building.