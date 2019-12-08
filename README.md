# convenient-vmbrowsing-guide  
This guide will walk your through setting up a light weight debian vm, with the sole purpose of using it for browsing the internet.  
The vm will be run at at boot, autologin a user and autostart firefox.  
This way it is almost as convenient as surfing the internet from your host system and you get all the security benefits  
browsing in a vm provides.
  
  
  
  
  
## create vm

* download virtualbox  
* download debian-xfce.iso: https://cdimage.debian.org/debian-cd/current/amd64/iso-cd/debian-10.2.0-amd64-xfce-CD-1.iso  

* create new vm in virtualbox, install debian on it:  
    * name virtual machine „debian for surfing“  
    * create user named „surfing“ while installing  
* (protipp: enable „skalierter modus“(scaled mode) with hostkey+c after starting vm)  
  
* install vm guest additions (if you run into an error, saying headers are not installed, run  
* sudo apt-get install build-essential linux-headers-uname -r dkms)  
  
* enable bidirectional drag and drop virtualbox → devices → drag and drop → bidirectional  
  
**do the following in the newly created vm___________________________________________________________**  

## make user „surfing“ sudoer  
* su -  
* usermod -aG sudo surfing  

## autostart firefox after booting  
* create file /etc/xdg/autostart/firefox.desktop  
* add following content to this file:  
  [Desktop Entry]  
  Encoding=UTF-8  
  Name=autostartFirefox  
  Exec=firefox  
  Terminal=false  
  Type=Application  
  
## autologin user „surfing“ when booting  
* addgroup --system nopasswdlogin  
* adduser surfing nopasswdlogin  
* uncomment in file /etc/lightdm/lightdm.conf:  
    greeter-hide-users=false  
    autologin-user=surfing  
    autologin-user-timeout=0  
* edit file /etc/pam.d/lightdm:  
    Before the @include common-auth line , add the following line :  
    auth sufficient pam_succeed_if.so user ingroup nopasswdlogin  
**___________________________________________________________________________________________**  
  
## make sure keyboard is not locked in virtualbox  
This is done so you can switch workspaces (i.e. ctrl+ tab) without having to disable keyboard-lock each time.  
* virtualbox → datei(file) → einstellungen(preferences) → eingabe(input?) → uncheck „autofang modus für tastatur“(the only checkbox visible..)  
  
## start at boot  
  
**for systems using xdg based desktopsoftware:**  
* create file: /etc/xdg/autostart/start-debian-for-surfing.desktop  
* add following content to this file:  
    [Desktop Entry]  
    Encoding=UTF-8  
    Name=start debian for surfing vm  
    Exec=VBoxManage startvm "debian for surfing"  
    Terminal=true  
    Type=Application  
  
Otherwise put VBoxManage startvm "debian for surfing" in a script and make it run at boot (after gui has loaded -> /etc/init.d/ wont work)  
