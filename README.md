# Rasp_samba_deluge
Server de partage de fichier et de téléchargement de torrent

## Instllation de la carte SD
- téléchargment de raspbian IMAGER: https://downloads.raspberrypi.org/imager/imager_latest.exe
  * Rasberry 4
  * selection rapi OS Lite 64Bits
  * Selection la carte SD
- configurer Image
  * nom : mars
  * ssh : oui
  * wifi : ou
- Ip scanner : https://www.advanced-ip-scanner.com/fr/download/
- Se conneceter en ssh

  ## Configurer le disque
Voire l'état du disque
```
lsblk
```
Créer repertoir, monter disque, creation entré dans /etc/fstav
```
sudo -s
mkdir /media/5to
mount /dev/sda1 /media/5to
echo "UUID=$(blkid --match-tag UUID | grep "/dev/sda1" | cut -d\" -f2) /media/5to $(blkid --match-tag TYPE | grep "/dev/sda1" | cut -d\" -f2) defaults,noatime 0 2 " >> /etc/fstab
mount -a 
```
## Installer samba
https://www.it-connect.fr/serveur-de-fichiers-debian-installer-et-configurer-samba-4/
```
apt-get install samba
systemctl enable smbd
```
Ajouter les ligne à la fin du fichier samba
```
cp /etc/samba/smb.conf /etc/samba/.smb.conf.bak
cat << ENDCONFiGSAMBA > /etc/samba/smb.conf
[global]
workgroup = WORKGROUP

[5to]
   path = /media/5to
   guest ok = no
   read only = no
   browseable = yes
   valid users = lune
ENDCONFiGSAMBA

```
restart service samba
```
systemctl restart smbd
```
creer group 5to ajouter uner utilisateur
```
adduser lune
smbpasswd -a lune
```
## Installer deluge
https://influence-pc.fr/01-04-2017-installation-de-deluge-sur-debian-8-comme-interface-web-de-telechargement-de-torrents
```
apt-get install deluged deluge-web deluge-console -y
adduser --system  --gecos "Deluge Service" --disabled-password --group deluge
```

Créez le fichier /etc/systemd/system/deluged.service :
```
[Unit]
Description=Deluge Bittorrent Client Daemon
After=network-online.target

[Service]
Type=simple
User=deluge
Group=deluge
UMask=000

ExecStart=/usr/bin/deluged -d

Restart=on-failure

# Configures the time to wait before service is stopped forcefully.
TimeoutStopSec=300

[Install]
WantedBy=multi-user.target
```
Puis exécutez :
```
systemctl start deluged
systemctl enable /etc/systemd/system/deluged.service
```

Créez le fichier /etc/systemd/system/deluge-web.service :
```
[Unit]
Description=Deluge Bittorrent Client Web Interface
After=network-online.target

[Service]
Type=simple

User=deluge
Group=deluge
UMask=027

ExecStart=/usr/bin/deluge-web

Restart=on-failure

[Install]
WantedBy=multi-user.target
```
Et enfin :
```
systemctl start deluge-web
systemctl enable /etc/systemd/system/deluge-web.service
```

### Configuration initiale

On passe en user deluge :
```
su --shell /bin/bash --login deluge
```
On créé un utilisateur par défaut pour le démon :
```
echo "deluge:deluge:10" >> ~/.config/deluge/auth
```
On autorise l’interface web à s’y connecter :
```
deluge-console "config -s allow_remote True"
deluge-console "config allow_remote"
```
et on redémarre la machine !

### Tips
Une fois installé, connectez vous sur l’interface avec les identifiants que vous avez choisi :

> http://<serveur>:8112




documentation
 apt-get install python python-twisted python-openssl python-setuptools intltool python-xdg python-chardet geoip-database python-libtorrent python-notify python-pygame python-glade2 librsvg2-common xdg-utils python-mako -y
 

mars

apt-get install python3 python3-twisted python3-openssl python3-setuptools intltool python3-xdg python3-chardet geoip-database python3-libtorrent python3-pygame librsvg2-common xdg-utils python3-mako -y
apt-get install git -y
git clone git://deluge-torrent.org/deluge.git
cd clone
git branch -a 
git checkout master
git pull
python setup.py install
python setup.py clean -a


Anexes
```
#echo "UUID=$(fdisk -l | grep -A 7 "/dev/sda" | grep "identifier" | cut -d' ' -f3) /media/5to $(df -Th | grep "/dev/sda1" | cut -d' ' -f7) defaults,noatime 0 2 " >> /etc/fstab
```
