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
nano /etc/samba/smb.conf
```
```
[partage]
   comment = Partage de données
   path = /media/5to
   guest ok = no
   read only = no
   browseable = yes
   valid users = partage
```

note
```
#echo "UUID=$(fdisk -l | grep -A 7 "/dev/sda" | grep "identifier" | cut -d' ' -f3) /media/5to $(df -Th | grep "/dev/sda1" | cut -d' ' -f7) defaults,noatime 0 2 " >> /etc/fstab
```
