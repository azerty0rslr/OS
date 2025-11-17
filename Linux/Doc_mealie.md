#  Projet selfhosting
Sur un serveur Debian 13 (trixie) nous devons installer un logiciel (Mealie) qui est un répertoire de recettes de cuisine numérisées.  
Projet réalisé par Manon ROUSSELIERE et Meven DESBOIS.  

## 1/ Installation du logiciel
Sur notre serveur Debian, nous avons commencé par installer les dépendances
- python3 avec pip et venv
- Node.JS
- Postgressql
- Yarn  
clone le projet : git clone https://github.com/mealie-recipes/mealie/

1. Installer le logiciel sur le serveur, le compiler à partir des sources directement  
2. Valider le bon fonctionnement du logiciel et de toutes ses fonctionnalités  
3. Automatiser le processus d’installation  

## 2/ Backup

1. En utilisant le logiciel restic , créer un script qui archive les données importante du service  
2. Configurez le logiciel cron pour qu’il exécute ce script toutes les heures  
3. En utilisant l’utilitaire rclone , transférez le backup sur un serveur distant (ex: Google Drive)  

## 3/ Sécurité
1. Mettre en place les règles de pare-feux pour n’accepter que le traffic sur le port de votre service  
2. Configurez fail2ban pour que les tentatives de bruteforce (ex: login failed 5 fois de suite) soient repérées  
 De même, repérez l’énumération web (lorsqu’un attaquant essaie plein de pages au hasard)  

## 4/ Monitoring
Mettre en place un outil de monitoring de votre service (ex: Grafana), et exporter des métriques depuis le serveur vers ce service.  
Ce service de monitoring ne doit pas être exposé au réseau externe. On y accèdera par port forwarding en SSH.  

### Documentation utilisé 
Pour le grand 1, nous avons utilisé la vidéo https://www.youtube.com/watch?v=rFPbOJc-pQY&t=52s  
https://gist.github.com/aman207/40f97b9feead389d1378a7ad2cee0992

su - : pour être sudo sans admin
