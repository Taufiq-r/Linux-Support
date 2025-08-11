# Linux-Support

## Update 
* `sudo dnf -y update`

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
