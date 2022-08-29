Install and Configure Ubuntu
===

# 1. Install system
- Note `UEFI`, `Legacy` things in BIOS setting, as they are related to disk partitioning method. `UEFI` needs a `efi` partition.
- `/boot` should not be too small (e.g., <512Mb), or system updates sometimes fail due to insufficient space in `/boot`. 

# 2. Install Chinese sougou-pinyin input method
- Following the [offical site](https://shurufa.sogou.com/linux) to configure system, downloand and install sougou-pinyin.
- Note the required libary that is not listed in the offical site `qml-module-gsettings1.0`.
  Install it with:
  ```bash
  sudo apt install qml-module-gsettings1.0
  ```
- If it cannot work properly, check the lack of libraries by running `fcitx-autostart` in a terminal. Its output/logs will show which libraries are missing.


# 3. Configure gnome's appearance with `Tweak`

- **Instal Tweak**
  ```bash
  sudo apt install gnome-tweak-tool
  sudo apt install gnome-shell-extensions
  ```
- **Adjust theme**
  - Open `Tweaks`-->`Appearance`-->`Applications`, select one of the theme from dropdown list.
  - New themes can be downloaded from [GTK3/4 Themes](https://www.gnome-look.org/browse?cat=135).
  - New themes should be place in the folder `.themes` in users' home directory.
  - Here is [an adjusted theme](https://github.com/sheng09/install-ubuntu/blob/main/gnome-professional-solid-40.1-WANG.tgz) based on [Prof-Gnome-theme](https://www.gnome-look.org/p/1334194).
- **Transparent top bar**.
  - Install one more gnome extension: [Transparent Top Bar (Adjustable transparency)](https://extensions.gnome.org/extension/3960/transparent-top-bar-adjustable-transparency/).
  - Adjust `Extensions`-->`Transparent Top Bar (Adjustable transparency)`.
- **Transparent dock**
  - Install one more more gnome extension: [dash-to-dock](https://extensions.gnome.org/extension/307/dash-to-dock/).
  - Adjust `Extensions`-->`Dash to dock`. (`Intelligent autohide` can be good.)
- **Other adjustments within Tweak**
  - `Extensions`-->`Horizontal workspaces`.
  - `Workspaces`-->`Number of Workspaces: 3`.

# 4. Configure fonts
- **Font-manager**
  ```bash
  sudo apt install font-manager
  ```
- Search for and get more font `.ttf` files, and add them with `font-manager`.
- Many fancy fonts can be found [here](https://www.dafont.com/theme.php?cat=103) (e.g., [hacker](https://www.dafont.com/hack.font)).

