# Linux-Support

## Update 
* `sudo dnf -y update`
* 

## Sharing File with Windows using Samba

* install samba on fedora
```
sudo dnf install samba
```
*set samba password
```
sudo smbpasswd -a
```
*setting samba config
```
sudo nano /etc/samba/smb.conf
```
*add this at end line
```
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
  ```
  firewall-cmd --add-service=samba
  firewall-cmd --runtime-to-permanent
  ```
*run samba
```
sudo systemctl enable smb.service
sudo systemctl start smb.service
sudo systemctl status smb.service

```
* make sure has setting network on linux match with windows
