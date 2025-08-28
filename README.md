# Linux-FEDORA42-Software/Tools/ Installation Source Command Line

* Update 
* ```
  sudo dnf -y update
  ```
* Install DNF Utillities  
* ```
  sudo dnf install -y dnf-plugins-core 'dnf-command(config-manager)'
  ```

## Add GPG & SSH Key
## GPG
* Create new GPG Key
* ``` gpg --full-generate-key ```
* Select what kind of key you want:
   (1) RSA and RSA
   (2) DSA and Elgamal
   (3) DSA (sign only)
   (4) RSA (sign only)
   (9) ECC (sign and encrypt) *default*
  (10) ECC (sign only)
  (14) Existing key from card
* Choose bits long *Recomend 4096
* specify how long the key should be valid.
         0 = key does not expire
      <n>  = key expires in n days
      <n>w = key expires in n weeks
      <n>m = key expires in n months
      <n>y = key expires in n years
* GnuPG needs to construct a user ID to identify your key.
* Enter your name, email, and Comment (optional)
* Change (N)ame, (C)omment, (E)mail or (O)kay/(Q)uit?
* Enter passphrase
* ``` gpg --list-secret-keys --keyid-format=long ``` for see created GPG Keys 
* ``` taufiq-r@fedora:~$ gpg --armor --export 'your id' ``` for export your GPG Key 'your id'
* Done
## SSH
* Create new SSH Key
* ssh-keygen -t rsa -b 4096 -C 'your email'
* Enter file in which to save the key by default (/home/taufiq-r/.ssh/id_rsa) and press Enter
* You can add passphrase when prompted, Enter passphrase for "/home/taufiq-r/.ssh/id_rsa" (empty for no passphrase)
* Enter same passphrase again:
* Your identification has been saved in /home/taufiq-r/.ssh/id_rsa
* Your public key has been saved in /home/taufiq-r/.ssh/id_rsa.pub
* for view your SSH Key type ``` cat ~/.ssh/id_rsa.pub``` and you will see the SSH Key you have created 


## Reset forgot su password
* Reboot and enter grub menu by press 'e' and select first Fedora Linux workstation
* add ``` init=/bin/bash ``` after line rhgb quiet -> example: ``` rhgb quiet init=/bin/bash ```
* press ctrl + x
* type ``` passwd ``` when bash terminal appear and type your new su password
* type ``` touch /.autorelabel ```
* type ``` exec /sbin/init ```
* note: if error passwd: "authentication token manipulation error" try to remount root partition by type this ``` sudo mount -o remount, rw / ``` and try passwd again.
  
## Install GNome Tweak + Extension
* ```
  sudo dnf install gnome-tweaks
  ```
# Instal extensions for customize GNOME Theme
* ```
  sudo dnf install gnome-extensions-app
  ```
* ```
  sudo dnf install gnome-shell-extension-user-theme
  ```
* Search theme on terminal
* ```
  dnf search gtk | grep theme
  ```
## Enable right click mouse / touchpad
* Enable right click
* ```
  gsettings set org.gnome.desktop.peripherals.touchpad click-method 'areas'
  ```
* Default
* ```
  gsettings set org.gnome.desktop.peripherals.touchpad click-method 'fingers'
  ```
  

## Install Docker
* Create Docker repo configuration file on DNF
* ```
  sudo nano /etc/yum.repos.d/docker-ce.repo
  ```
* Add this
* ```
  [docker-ce-stable]
  name=Docker CE Stable - $basearch
  baseurl=https://download.docker.com/linux/fedora/42/$basearch/stable
  enabled=1
  gpgcheck=1
  gpgkey=https://download.docker.com/linux/fedora/gpg
  ```
* Install Docker
* ```
  sudo dnf install -y docker-ce docker-ce-cli containerd.io
  ```
* Enable and Run Docker
* ```
  sudo systemctl start docker
  sudo systemctl enable docker
  ```
* (optional) add usergroup docker
* ```
  sudo usermod -aG docker 'user'
  ```
* ```
  newgrp docker
  ```
## Install Gitlab *FEDORA-42*
* Create Gitlab folder
* ```
  sudo mkdir -p /srv/gitlab/config /srv/gitlab/logs /srv/gitlab/data
  sudo chmod -R 777 /srv/gitlab/
  ```
* run this
* ```
  docker run --detach \
  --hostname gitlab.'xyz'.com \
  --publish 443:443 --publish 80:80 --publish 22:22 \
  --name gitlab \
  --restart always \
  --volume /srv/gitlab/config:/etc/gitlab \
  --volume /srv/gitlab/logs:/var/log/gitlab \
  --volume /srv/gitlab/data:/var/opt/gitlab \
  gitlab/gitlab-ce:latest
  ```
* edit host config
* ```
  sudo nano /etc/hosts
  ```
* add this at end line
* ```
  127.0.0.1       gitlab.'xyz'.com
  ```
* get password for login gitlab as root
* ```
  sudo docker exec gitlab grep 'Password:' /etc/gitlab/initial_root_password
  ``` 
  
## Sharing File with Windows using Samba

* install samba on fedora
* ```
  sudo dnf install samba
  ```
* set samba password
* ```
  sudo smbpasswd -a
  ```
* setting samba config
* ```
  sudo nano /etc/samba/smb.conf
  ```
* add this at end line
* ```
  [Share]

        #specify shared directory
        path = /home/taufiq-r/share
        #allow write
        writable = yes
        #allow guest user
        guest ok = yes
        #looks all as guest user
        guest only = yes
        #set permission 777 when file created
        force create mode = 777
        #set persmission 777 when folder created
        force directory mode = 777
  ```
* allow firewall
  * ```
     firewall-cmd --add-service=samba
     firewall-cmd --runtime-to-permanent
    ```
* run samba
* ```
  sudo systemctl enable smb.service
  sudo systemctl start smb.service
  sudo systemctl status smb.service
  ```
* make sure has setting network on linux match with windows

## Install Discord
* add RPM Fusion Repository
* ```sudo dnf install https://download1.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm```
* ```sudo dnf install https://download1.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm```
* Install Discord via Terminal by type ``` sudo dnf install discord ```
  OR
## Install Discord from flatpak

* add repository if not exist
  * ```
    sudo flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
    ```
* Install flatpak
  * ```
     sudo dnf install flatpak -y
    ```
* Install discord
  * ```
    flatpak install flathub com.discordapp.Discord -y

    ```
## Install yt-dlp for download udemy course 
* INSTALL WITHOUT VENV
* ```
  python3 -m pip install -U yt-dlp --user
  ```
* Add PATH
* ```
  ~/.local/bin ke ~/.bashrc
  ```
* Run venv
* ```
  source ~/.bashrc
  ```
* Check Version
* ```
  yt-dlp --version

  ```
* Alternative
*  ```
   ALTERNATIF -> sudo curl -L https://github.com/yt-dlp/yt-dlp/releases/latest/download/yt-dlp -o /usr/local/bin/yt-dlp

   ```
* give permission
*  ```
   sudo chmod a+rx /usr/local/bin/yt-dlp
   ```
* Run
*  ```
   yt-dlp \
          --cookies ~/Your PATH/cookies.txt \
          --user-agent "Mozilla/5.0 (X11; Linux x86_64; rv:128.0) Gecko/20100101 Firefox/128.0" \
          --referer "https://www.udemy.com" \
          --write-auto-sub \
          --sub-lang en \
          --convert-subs srt \
          "https://www.udemy.com/course/Name Course"
   ```
## Install VSCode

* Import GPG keys microsoft
* ```
  sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc
  ```
* Add VSCode repository if not exist
* ```
  sudo sh -c 'echo -e "[code]\nname=Visual Studio Code\nbaseurl=https://packages.microsoft.com/yumrepos/vscode\nenabled=1\ngpgcheck=1\ngpgkey=https://packages.microsoft.com/keys/microsoft.asc" > /etc/yum.repos.d/vscode.repo'
  ```
* Run install command
* ```
  sudo dnf install code
  ```
* Run VSCode by type 'code' in your terminal

## Install MS Fonts like times new roman

* Install Microsoft Fonts via msttcore-fonts-installer

* Install cabextract for extracting Microsoft font files, and fontconfig for managing and customizing font access. Most Fedora installations will have these by default, but it’s good practice to check and install any missing ones.
* ```
  sudo dnf install curl cabextract xorg-x11-font-utils fontconfig
  ```
* Proceed to download and install the Microsoft Core Fonts package using the following command:
* ```
  sudo rpm -i https://downloads.sourceforge.net/project/mscorefonts2/rpms/msttcore-fonts-installer-2.6-1.noarch.rpm
  ```
* after installing, check font with open Fonts Applications from desktop
* search font like times new roman, arial etc
* check LibreOffice
* Done

## Flash MIUI Fastboot ROM via terminal
* Install Android-tools
* ```
  sudo dnf install android-tools
  ```
* extract Miui ROM, you can find any Fastboot ROM at https://xmfirmwareupdater.com
* ```
  tar -xzvf 'ROM_Name'.tgz
  ```
* move to ROM Folder
* ```
  cd 'ROM_Name'
  ```
* 
