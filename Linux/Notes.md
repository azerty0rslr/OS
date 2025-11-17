# Commande 
Ctrl + C : commande pour arrêter le script
# Ligne
**shebang :** #!/bin/bash

driver : code réalisé par le fabriquant, avec les spécificités du matériel
kernel : maître d'orchestre du PC, s'occupe de la mise en place des interfaces pour les driver, dialogue avec le hardware.
Application et système d'exploitation sont non privilégié, si veulent privilège 
Kernel, driver et hardware ont les privilèges 
syscall
cd : retourne à la page home de l'utilisateur
path : 

Philosophie linux : tout est un fichier
/run : stocke les fichiers temporaires
OS (système d'expoitation) : ensemble d'outils/de logiciels permettant d'utiliser ergoniquement le PC.
BASH : Bourne Again Shell, un shell permettant d'exécuter les petits outils de l'OS 

Script : régupérer un arg ($1arg,$2arg), tout les arg $@
homr 

# Le système d'exploitation
Processus d'amorçage : boot = démarrage, initialisation de l'ordinateur
Démarrage en 4 temps : 
1) BIOS : logiciel d'initialisation du matériel situé dans la ROM (Read Only Memory) - initialise notamment la RAM, puis recherche le MBR -partition numéro 1 (en FAT32)
2) Bootloader : 1e exécution de lancée le bootloader, 1e kernel, peux avoir des configurations (routage de paquets, boot réseaux ...), configure kernel, stockée sur le MBR (Master Boot Record)
3) Kernel : boot exécuté par le bootloader (/boot) il est situé dans /boot dans la même partition que bootloader, on peut modifier les options du bootloader, changer 
4) Init script : logiciel lancé par le kernel lorsqu'il a terminé son initialisation situé dans /sbin/init

## Services et démons
Logiciel (appelé serveur) utilise des services en tâche de fond appelés daemon (logiciel/ services finissent par un d pour le signifier)
En général, on utilise systemd pour gérer ces services (dépendances, lancements conditionnels). Tous ses services sont configurables. 

## Gestion des disques 
Chaque disque aura un label basé sur son type (SD - disque dur/clé USB, nvme - disque NVMe, mmcblk - disque eMMC/Carte SD). Chaque disque aura un label unique (sda, sdb, ...; nvme0n1, nvme1n1, ...). Chaque disque peut être séparé en partitions, une manière de séparer les différents usages. 
L'organisation des données sur chaque disque dépend du logiciel utilisé pour les organiser, appelé filesystem (système de fichiers : sorte d'annuaire).
Linux supporte beaucoup de filesystem différents (ext4, ntfs, apfs, btrfs ...)

## Gestion de processus
Un processus est une instance d'un programme en cours d'exécution. Chaque processus a un identifiant unique (PID : process ID). On peut faire différentes oppératioons grâce au PID tel que ps (liste processus), top/htop (afficher les processus), kill (signal au processus, le processus n'est pas obligé d'écouter le signal), nice/renice (changer la priorité d'un processus).

### Signaux 
sigterm, sigkill, sighup

## Bibliothèques partagées
A partir d'un code de base utilisant une bibliothèque externe, un logiciel que l'on a compilé, transformé en un exécutable, peut être lié de 2 manières différentes : liaison statique (binaire contient toutes les bibliothèques nécessaires), liaison dynamique (binaire utilise des bibliothèques externes (.so) chargées à l'exécution). Ces bibliothèques partagées permettent une taille réduite des binaire, mises à jour centralisées des bibliothèques, économie de mémoire mais risque de dépendances externes à gérer, risque de conflits de versions.  

Sous Windows, dll (dynamicly loaded library), sous linux .so (shared object). Elles sont stockées dans des répertoires standard : /lib, /lib64, /usr/lib, /usr/local/lib. 
Commandes : 

## Gestion des dépendances
Puisqu'on a des lionsons dynamiques nécessitants des gestions de dépendances.
Pour pouvoir lancer un logiciel nécessitant des bibliothèques partagées au runtime, il faut donc installer ses dépendances et les dépendances de ses dépendances. Pour cela, un package manager est essentiel : installe, MAJ et supprime les logiciels, intègre le logiciel à l'OS, gère les dépendances, évite les conflits de versions. Attention les distributions n'utilise pas le même manager (ex : debian utilise apt alors que fedora utilise dnf).
