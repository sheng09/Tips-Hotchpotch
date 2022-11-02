A Mixture of Tips
===
<!-- TOC -->

- [A Mixture of Tips](#a-mixture-of-tips)
- [1. Install and Configure Ubuntu](#1-install-and-configure-ubuntu)
  - [1.1 Install system](#11-install-system)
  - [1.2 Install Chinese sougou-pinyin input method](#12-install-chinese-sougou-pinyin-input-method)
  - [1.3 Configure gnome's appearance with `Tweak`](#13-configure-gnomes-appearance-with-tweak)
  - [1.4 Configure fonts](#14-configure-fonts)
- [2. Setup Different Servers](#2-setup-different-servers)
- [3. Python Things](#3-python-things)
  - [3.1 Manage Python modules with `pip3`](#31-manage-python-modules-with-pip3)
- [X. Some problems and answers](#x-some-problems-and-answers)
  - [X.1 Run `pyvista` from SSH Remote Server](#x1-run-pyvista-from-ssh-remote-server)

<!-- /TOC -->

# 1. Install and Configure Ubuntu

## 1.1 Install system
- Note `UEFI`, `Legacy` things in BIOS setting, as they are related to disk partitioning method. `UEFI` needs an `efi` partition.
- `/boot` should not be too small (e.g., <512Mb), or system updates sometimes fail due to insufficient space in `/boot`. 

## 1.2 Install Chinese sougou-pinyin input method
- Following the [offical site](https://shurufa.sogou.com/linux) to configure system, downloand and install sougou-pinyin.
- Note the required libary that is not listed in the offical site `qml-module-gsettings1.0`.
  Install it with:
  ```bash
  sudo apt install qml-module-gsettings1.0
  ```
- If it cannot work properly, check the lack of libraries by running `fcitx-autostart` in a terminal. Its output/logs will show which libraries are missing.


## 1.3 Configure gnome's appearance with `Tweak`

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
  - Disable the ubuntu built-in dock in case of two overlapping docks.
     ```bash
     gnome-extensions disable ubuntu-dock@ubuntu.com
     ```
- **Other adjustments within Tweak**
  - `Extensions`-->`Horizontal workspaces`.
  - `Workspaces`-->`Number of Workspaces: 3`.

## 1.4 Configure fonts
- Manage fonts with **Font-manager** as a sudoer.
  - Instal `font-manager`:
     ```bash
     sudo apt install font-manager
     ```
  - Search for and get more font `.ttf` files, and add them with `font-manager`.
  - Many fancy fonts can be found [here](https://www.dafont.com/theme.php?cat=103) (e.g., [hacker](https://www.dafont.com/hack.font)).
- Add/manage fonts as an user.
  - Add
    ```bash
    mkdir ~/.local/share/fonts # create this directory if it does not exist
    mv font_name_.ttf ~/.local/share/fonts/
    fc-cache -f -v  #clear and regenerate your font cache
    ```
  - Check if a font is installed.
    ```bash
    fc-list | grep Arial # an example for checking for Arial
    ```
- Update **matplotlib** cache to enable using new fonts.
    ```bash
    rm ~/.cache/matplotlib -rf
    ```

# 2. Setup Different Servers 
- **OpenSSH server**

  Run the commands below to install and enable openssh server.
  ```
  sudo apt-get install openssh-server
  sudo systemctl status ssh  # check the status
  sudo systemctl enable ssh  # enable and start ssh
  sudo systemctl start ssh
  #
  sudo apt install net-tools
  ifconfig  # check the IP address of the machine
  ```
- **Apache web server** for sharing files
  - Install `apache2`:
    ```bash
    sudo apt install apache2
    ```
  - Create a directory `foldername_whatever/` in home, and then create a symbolic link to it in `/var/www/html`. Finally, one can access `$ip$/toshare` from a web browser.
    ```bash
    cd ~
    mkdir foldername_whatever
    cd /var/www/html
    sudo ln -s ~/foldername_whatever .
    # Then one can access $ip$/foldername_whatever, in which $ip$ is the IP/address of the machine.
    ```
  - Restart/stop apache2 server.
    ```bash
    sudo service apache2 stop
    sudo service apache2 restart
    sudo service apache2 reload   # etc
    ```
- **Jupyter-lab server**
   - Install `jupyterlab` via `pip3`.
     ```bash
     pip3 install jupyterlab
     ```
   - Enable and configure jupyterlab server.
     ```bash
     jupyter server --generate-config
     jupyter server password  # set the password
     nohup jupyter-lab --port $XXXX$ --no-browser > ~/.jupyter/nohuo.log & # replace XXXX to four digits, (e.g., 9832)
     ```
   - Access jupyterlab server on local machine. Open `http://localhost:$XXXX$` in a web browser, where $XXXX$ is the four digits.
   - Access jupyterlab server on a remote terminal.
     ```bash
     ssh -N -L $YYYY$:localhost:$XXXX$ username@server_address
     ```
     in which `$XXXX$` is the previous four digits set on the server, and `$YYYY$` is another four independent digits set on the remote terminal.
     Then, one can access on the remote terminal by opening `http://localhost:$YYYY$` in a web browser.

# 3. Python Things

## 3.1 Manage Python modules with `pip3`
  - Several commands for managing/fixing pip3 modules and their dependency:
    ```bash
    # Specify version when install a module
    # The following example specify version 1.22
    pip3 install numpy==1.22
    
    # Ignore the existed module
    # The following example will ignore the installed matplotlib in /usr/local/...,
    # and install the matplotlib to ~/.local/....
    pip3 install --ignore-installed --user matplotlib
    
    # 
    ```
    
# X. Some problems and answers

## X.1 Run `pyvista` from SSH Remote Server
  - Some errors when running `pyvista` via ssh from a remote server:
    ```bash
    libGL error: No matching fbConfigs or visuals found
    libGL error: failed to load driver: swrast
    ```
    Solution
    ```bash
    # Set this on the remote server
    export DISPLAY=:0.0 glxinfo
    ```
