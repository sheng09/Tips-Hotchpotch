Install and Configure Ubuntu
===

# 1. Install system
- Note `UEFI`, `Legacy` things in BIOS setting, as they are related to disk partitioning method. `UEFI` needs an `efi` partition.
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
  - Disable the ubuntu built-in dock in case of two overlapping docks.
     ```bash
     gnome-extensions disable ubuntu-dock@ubuntu.com
     ```
- **Other adjustments within Tweak**
  - `Extensions`-->`Horizontal workspaces`.
  - `Workspaces`-->`Number of Workspaces: 3`.

# 4. Configure fonts
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
# 5. Setup servers 
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


