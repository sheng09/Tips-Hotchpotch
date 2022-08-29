# Install and Configure Ubuntu

# 1. Install system
- Note `UEFI`, `Legacy` things in BIOS setting, as they are related to disk partitioning method. `UEFI` needs a `efi` partition.
- `/boot` should not be too small (e.g., <512Mb), or system updates sometimes fail due to insufficient space in `/boot`. 

# 2. Install Chinese Sougou-pinyin input method
- Following the [offical site](https://shurufa.sogou.com/linux) to configure system, downloand and install sougou-pinyin.
- Note the required libary that is not listed in the offical site `qml-module-gsettings1.0`.
  Install it with:
  ```bash
  sudo apt install qml-module-gsettings1.0
  ```
- If it cannot work properly, check the lack of libraries by running `fcitx-autostart` in a terminal. Its output/logs will show which libraries are missing.





