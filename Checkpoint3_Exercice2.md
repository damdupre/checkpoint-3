## Partie 1 : Gestion des utilisateurs

### Q.2.1.1
Pour pouvoir créer un nouvel utilisateur , j'ai me suis d'abord mis en utilisateur  **root** , puis j'ai mis à jour la vm `apt update` puis installer sudo `apt install sudo` j'ai ensuiter pu créer mon utilisateur avec `sudo adduser dams` .  

### Q.2.1.2
Pour ce compte il faut définir un mot de passe fort (12 caractéré avec des minuscule de majuscule des chiffres et des caractéres spéciaux),il aut crée un répertoir personnel et gérer les droits d'accés au systeme, on peux aussi définir un quota de disque utilisé et mettre en place une journalisation des événements.

## Partie 2 : Configuration de SSH

### Q.2.2.1
Pour pouvoir désactiver **root** de l'accées ssh il faut ce rendre dans le fichier `/etc/ssh/sshd_confg` puis décommenter la ligne `PermitRootLogin yes` en remplacant le **yes** par **no** , on enregistre et on redémarre le service ssh avec `service ssh restart` .  

### Q.2.2.2
Dans le fichier de configuration ssh `/etc/ssh/sshd_config` j'ai ajouter une ligne `AllowUsers dams` qui permet d'autoriser que dams en SSH. 

### Q.2.2.3
J'ai d'abord généré une clé ssh avec `ssh-keygen`. 
Dans le fichier de config je modifie le ligne `PasswordAuthentication yes` par `PasswordAuthentication no`  puis je redemarre le service `systemctl restart ssh`.  

## Partie 3 : Analyse du stockage

### Q.2.3.1
Avec la commande `df -h` on obtient la liste des systeme de fichier monté.  
![fichier](https://github.com/damdupre/checkpoint-3/blob/main/images/fichier.png)

### Q.2.3.2

Avec la commande `lsblk` on peut voir qu'il il y un systeme raid et un systeme lvm .  
![stockage](https://github.com/damdupre/checkpoint-3/blob/main/images/info%20stockage.png)  

### Q.2.3.3
Aprés avoir ajouté un nouveaux disque   
![ajout dd](https://github.com/damdupre/checkpoint-3/blob/main/images/ajout%20dd.png)    
J'ai d'abord contrôler l'état du raid avec `sudo mdadm --detail /dev/md0`, je me suis aperçu qu'il manquait un disque au raid.    
Avec la commance `sudo mdadm --manage /dev/md0 --add /dev/sdb` j'ai ajouter le disque sdb au raid et la reconstruction c'est lancé.
![reconstruction](https://github.com/damdupre/checkpoint-3/blob/main/images/reconstruction.png)  
Puis je contôle de nouveaux l'état du raid  
![raid ok](https://github.com/damdupre/checkpoint-3/blob/main/images/raid%20ok.png)    
Le raid est de nouveaux opérationnel.    

### Q.2.3.4 
Je commence par contrôler le lvm avec `sudo vgdisplay`  
![lvm control](https://github.com/damdupre/checkpoint-3/blob/main/images/lvm%20control.png)  

Je créer un LV de 2G dans le VG cp3-vg .  
`sudo lvcreate -L 2G lv_bareos_storage cp3-vg`   
resultat .  
![lvcreate](https://github.com/damdupre/checkpoint-3/blob/main/images/lvcreate.png)    
J'ai formater ce LV en ext4 avec `sudo mkfs.ext4 /dev/cp3-vg/lv_bareos_storage`    
Puis j'ai modifier le fichier fstab pour pouvoir monter le LV automatiquement au demarrage `echo "/dev/cp3-vg/lv_bareos_storage /var/lib/bareos/storage ext4 defaults 0 2" | sudo tee -a /etc/fstab` .    

### Q.2.3.5
Il reste 1.79 Gib de disponible ..  

## Partie 4 : Sauvegardes

### Q.2.4.1
`bareos-dir ` est le composant central de bareos c'est lui qui centralise et coordonne les opérations de sauvegarde et de restauration .  

`bareos-sd` gére le stockage des sauvegardes.

`bareos-fd` est un composant, installé sur les clients il permet de lire les fichiers et de les envoyer à `bareos-sd` pour la sauvegarde.  

## Partie 5 : Filtrage et analyse réseau

### Q.2.5.1
Les régle appliquées sont :    

- rejeter les paquets invalide  
- accepter les paquets local (lo)  
- accepter les connexion ssh (port22)  
- accepter l'icmp (ping)    
![regle](https://github.com/damdupre/checkpoint-3/blob/main/images/regle.png)  

### Q.2.5.2
Les communication local sont permise.

### Q.2.5.3
Les communication avec des paquets invalide sont rejetées.

### Q.2.5.4
J'ai créé une nouvelle table 
```bash
sudo nft add table inet bareos
sudo nft add chain inet bareos input { type filter hook input priority 0 \; }
sudo nft add chain inet bareos output { type filter hook output priority 0 \; }
sudo nft add chain inet bareos forward { type filter hook forward priority 0 \; }
```
puis j'ai ajouter les régles 
```bash
sudo nft add rule inet bareos input tcp dport 9101 accept
sudo nft add rule inet bareos input tcp dport 9102 accept
sudo nft add rule inet bareos input tcp dport 9103 accept

sudo nft add rule inet bareos output tcp sport 9101 accept
sudo nft add rule inet bareos output tcp sport 9102 accept
sudo nft add rule inet bareos output tcp sport 9103 accept

sudo nft add rule inet bareos forward tcp dport 9101 accept
sudo nft add rule inet bareos forward tcp dport 9102 accept
sudo nft add rule inet bareos forward tcp dport 9103 accept
```
resulat   
![regle bareos](https://github.com/damdupre/checkpoint-3/blob/main/images/regle%20bareos.png)  

## Partie 6 : Analyse de logs

### Q.2.6.1
j'utilise la commande `udo grep -E "Failed password|Failed publickey|Failed none" /var/log/auth.log | tail -n 10 | awk '{print $1, $2, $3, $11, $13}'` 
voici le résultat 
```bash
dams@SRVLX01:~$ s
Sep 6 14:00:00 red 192.168.0.37
Sep 6 14:00:06 red 192.168.0.37
Sep 6 14:00:10 red 192.168.0.37
Sep 6 14:00:12 red 192.168.0.37
Sep 6 14:00:13 red 192.168.0.37
Sep 6 14:07:58 ; ;
Sep 6 14:08:29 ; ;
Sep 6 14:10:45 ; ;
Sep 6 14:10:52 ; ;
Sep 6 14:11:01 ; ;
dams@SRVLX01:~$

``` 
L'utilisateur **red** a essayer de ce connecter plusieur fois le 6 septembre entre 14:00:00 et 14:00:13 



