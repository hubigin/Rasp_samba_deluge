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
'''
lsblk
'''
Créer repertoir, monter disque, creation entré dans /etc/fstav
'''
sudo -s
mkdir /media/5to
mount /dev/sda1 /media/5to

'''
