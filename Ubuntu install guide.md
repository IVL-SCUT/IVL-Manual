# Ubuntu Install Guide

A simple installation guide for Ubuntu.

## Single Boot

`# 2024.03.26-2 10:06`

- Download the required version of Ubuntu from Tsinghua mirror: [mirrors.tuna.tsinghua.edu.cn/ubuntu-releases/](https://mirrors.tuna.tsinghua.edu.cn/ubuntu-releases/).
- Download Rufus from [rufus.ie](https://rufus.ie/), recommended to use portable version. Use Rufus to create a bootable USB drive, all the settings are default.
- Boot from the USB drive, install Ubuntu with the default settings. During the installation, recommended to choose the option to install third-party software for graphics and Wi-Fi hardware and additional media formats.
- After the installation, change the software source to Huaweicloud: `Software & Updates` -> `Ubuntu Software` -> `Download from` -> `Other` -> `China-huaweicloud`.
- Update the system: `sudo apt update && sudo apt upgrade`.
- Install the required software: `sudo apt install vim ssh dkms -y`.

Optional:

- Set the password for root: `sudo passwd root`.
- Install Anaconda.
  - Download the installer from [mirrors.tuna.tsinghua.edu.cn/anaconda/archive/](https://mirrors.tuna.tsinghua.edu.cn/anaconda/archive/) with `wget https://mirrors.tuna.tsinghua.edu.cn/anaconda/archive/Anaconda3-VERSION-Linux-x86_64.sh`.
  - Install with `bash Anaconda3-VERSION-Linux-x86_64.sh`.
    - Press `Enter` to read the license.
    - Recommend to install in `/opt/anaconda3`.
    - Input `yes` to initialize conda.
  - For other users to use Anaconda, add the following line to `~/.bashrc`:

    ```bash
    export PATH="/opt/anaconda3/bin:$PATH"
    ```

    Then run `source ~/.bashrc && conda init`.

## Dual Boot with Windows

`# 2024.04.14-7 13:45`

Just follow the guide: [How to Set Up a Dual Boot with Ubuntu and Windows](https://gcore.com/learning/dual-boot-ubuntu-windows-setup/).

A more detailed guide with disk partitioning: [How to Install Ubuntu Alongside Windows 10](https://itsfoss.com/install-ubuntu-1404-dual-boot-mode-windows-8-81-uefi/).
