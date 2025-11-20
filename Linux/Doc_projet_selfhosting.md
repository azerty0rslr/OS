#  Projet selfhosting
Sur un serveur Debian 13 (trixie) nous devons installer un logiciel, nous avions choisi le logiciel Mealie qui est un répertoire de recettes de cuisine numérisées. Suite à des problèmes expliqués dans la doc, nous sommes passés sur le logiciel Ente, une plateforme de partage de photos cryptée de bout en bout (alternative à Google Photos, Apple Photos). Enfin, suite à des problèmes également expliqués dans la doc, nous avons dû passer sur le logiciel Forgejo, un serveur de forge git.  
Projet réalisé par Manon ROUSSELIERE et Meven DESBOIS.  

# 1/ Installation du logiciel
## Objectifs :
    1. Installer le logiciel sur le serveur, le compiler à partir des sources directement  
    2. Valider le bon fonctionnement du logiciel et de toutes ses fonctionnalités  
    3. Automatiser le processus d’installation 
## Jour 1 : 
### Mealie  
La documentation nous donne uniquement un dockerfile que nous avons essayer de traduire. Voici les étapes que nous avons suivis :  

#### 1/ Instalation des dépendances et clone du projet  
Sur notre serveur Debian, nous avons commencé par installer les dépendances :  
    - python3 avec pip et venv  
    - Node.JS  
    - Curl  
    - Postgressql  
    - Yarn  

Clôner le projet : ```git clone https://github.com/mealie-recipes/mealie/```  
  
#### 2/ Compiler le front-end de l'application
Utilisation de yarn :  
    - ```yarn install``` pour installer les dépendances  
    - ```yarn generate``` pour lancer le script de compilation  

#### 3/ Créer les variables d'environnement 

```bash
export VENV_PATH="/opt/mealie"
export NLTK_DATA="/nltk_data/"
export APP_PORT=9000
export PYTHONUNBUFFERED=1 \
export PYTHONDONTWRITEBYTECODE=1 \
export PIP_NO_CACHE_DIR=off \
export PIP_DISABLE_PIP_VERSION_CHECK=on \
export PIP_DEFAULT_TIMEOUT=100 \
export VENV_PATH="/opt/mealie"
````

#### 4/ Créer un environnement virtuel Python avec venv pour installer uv et uvicorn

```bash
# Création de l'environnement
python3 -m venv myenv
# Rentrer dans l'environnement
source myenv/bin/activate
# Installer UV
pip install uv
pip install uvicorn
```

```bash
# Copier les fichiers
cp uv.lock mealie/uv.lock
cp pyproject.toml mealie/pyproject.toml
cp mealie mealie/mealie
uv build --out-dir dist

uv export --no-editable --no-emit-project --extra pgsql --format requirements-txt --output-file dist/requirements.txt \
    && MEALIE_VERSION=$(python -c "import tomllib; print(tomllib.load(open('pyproject.toml', 'rb'))['project']['version'])") \
    && echo "mealie[pgsql]==${MEALIE_VERSION} \\" >> dist/requirements.txt \
    && pip hash dist/mealie-${MEALIE_VERSION}-py3-none-any.whl | tail -n1 | tr -d '\n' >> dist/requirements.txt \
    && echo " \\" >> dist/requirements.txt \
    && pip hash dist/mealie-${MEALIE_VERSION}.tar.gz | tail -n1 >> dist/requirements.txt
```
Le reste de la compilation se fait en utilansant une wheel dans l'environnement virtuel :  
C'est un fichier qui contient le code prêt a être compiler et les données associées.  
  
La compilation ne fonctionne pas car le dockerfile a plusieurs stages qui créent des images différentes pour les différentes étapes de compilation et alléger l'application à la fin de la compilation.  
Les liens entre les différentes images sont gérés par Docker et nous n'avons pas réussi à tous les comprendre.  
Le projet était trop difficiles pour nous et nous avons pris la décision de le changer.  

## Jour 2 :
### Ente 
La documentation nous donne les étapes à suivre pour build le projet depuis les sources et installer les dépendances :  
```bash
sudo apt update
sudo apt install nodejs npm
npm install --global yarn
sudo apt-get install rpm
git clone https://github.com/ente-io/ente
cd ente/desktop

# installation des dépendances
yarn install
yarn build
```
Problème sur les VM, les espaces de stockage sont arrivés à saturation, nous avons donc dû migrer dans le labo et recommencer.  
Nouveau problème, en arrivant à l'étape ```yarn install``` nous avons les erreurs suivantes : ```Request "https://yarnpkg.com/latest-version" finished with status code 500```, ```Request failed "500 Internal Server Error"```.  
Le serveur yarn est down. Nous devons donc attendre pour poursuivre notre TP.  

<img width="1495" height="826" alt="image" src="https://github.com/user-attachments/assets/094ba610-4696-4a70-866d-699e75692d4f" />  

Nous avions une erreur avec le yarn install qui essayait d'ouvrir ssh://github.com nous avons donc dû le remplacer par https://github.com
Pour lancer l'image de l'app, nous devons utiliser la commande (avec les droits) : ```sudo ./root/ente/desktop/dist/ente-1.7.16-beta-x86_64.AppImage -no-sandbox``` pour que la commande soit plus simple d'accès nous l'avons bougé d'endroit avec la commande ```sudo mv ./root/ente/desktop/dist/ente-1.7.16-beta-x86_64.AppImage /home/vboxuser/
```. Pour lancer l'image de l'app il faut désormais faire : ```./ente-1.7.16-beta-x86_64.AppImage```  
  
<img width="654" height="588" alt="image" src="https://github.com/user-attachments/assets/9527cd63-f82e-4781-8be5-a25624b3afee" />  
  
## Jour 3 :
### Installation du serveur de Ente - Museum
La documentation du site Ente nous donne les étapes à suivre pour l'installation du serveur : 
```bash
# Install Go
sudo apt update && sudo apt upgrade
sudo apt install golang-go

# Install PostgreSQL et libsodium
sudo apt install postgresql
sudo apt install libsodium23 libsodium-dev

sudo systemctl enable postgresql
sudo systemctl start postgresql

sudo systemctl status postgresql

# Install pkg-config
sudo apt install pkg-config

# Install npm and Node
sudo apt install npm nodejs

# Git, caddy, Object Storage

# Clone du git si pas encore fait
git clone https://github.com/ente-io/ente

# Change into server directory, where the source code for Museum is
# present inside the repo
cd ente/server

# Install the needed dependencies
go mod tidy

# Build the server
go build cmd/museum/main.go

# Create museum.yaml
cp config/example.yaml ./museum.yaml

# Run the server (http://localhost:8080)
./main
<img width="744" height="459" alt="image" src="https://github.com/user-attachments/assets/b9c903ce-74ba-40fb-9df0-c39db8c3555f" />
```
Nous avons eu un problème pour l'exécution du ```./main```. En effet museum.yaml n'est pas correctement remplis. 
```bash
# Modifier identifiants postgres
sudo -i -u postgres

psql

postgres=# ALTER USER postgres PASSWORD 'postgres';
postgres=# CREATE DATABASE ente_db;

```
Et modifier museum.yaml  
<img width="294" height="124" alt="image" src="https://github.com/user-attachments/assets/34f9bef3-a39e-4a64-91c4-7bebde386722" />  

Par la suite en réexécutant le ```./main``` nous avons l'erreur suivante dans le terminal : ```WARN[0009]main.go:1130 urlSanitizer Unknown API: /```. Et pas d'affichage sur http://localhost:8080/.  
<img width="738" height="332" alt="image" src="https://github.com/user-attachments/assets/070fd623-0f0d-4fbd-961d-7a05adfac925" />  
  
L'erreur venait de parcePricingFile, tout les fichiers étaient reliés avec les serveurs de Ente. Puisque le nombre de fichiers à modifier était trop important nous avons donc dû recommencer un nouveau projet.


### Forgejo
La documentation nous donne les étapes à suivre installer le projet :
```bash
# Download
wget https://codeberg.org/forgejo/forgejo/releases/download/v13.0.2/forgejo-13.0.2-linux-amd64
chmod +x forgejo-13.0.2-linux-amd64
```
#### Verify GPG signature
#### d'après la doc : should be downloaded every time to make sure the latest version is used.
```bash
gpg --keyserver keys.openpgp.org --recv EB114F5E6C0DC2BCDD183550A4B61A2DC5923710
wget https://codeberg.org/forgejo/forgejo/releases/download/v13.0.2/forgejo-13.0.2-linux-amd64.asc
gpg --verify forgejo-13.0.2-linux-amd64.asc forgejo-13.0.2-linux-amd64
```
#### Copy the dowload Forgejo to /usr/local/bin/
```bash
sudo cp forgejo-x.y.z-linux-amd64 /usr/local/bin/forgejo
sudo chmod 755 /usr/local/bin/forgejo
```

#### Install git
```bash
sudo apt install git git-lfs

# Créer un git utilisateur, remplacer git 
sudo adduser --system --shell /bin/bash --gecos 'Git Version Control' \
  --group --disabled-password --home /home/git git
```
#### Create Forgejo directories
```bash
# Forgejo utilise et définit les permissions d’accès de manière appropriée :
sudo mkdir /var/lib/forgejo
sudo chown git:git /var/lib/forgejo && sudo chmod 750 /var/lib/forgejo

# Configuration de Forgejo :
sudo mkdir /etc/forgejo
sudo chown root:git /etc/forgejo && sudo chmod 770 /etc/forgejo

# On ne set up pas de database car on considèrera SQlite suffisant (très bien pour environ 10 users)
```
#### Install systemd service for Forgejo
```bash
sudo wget -O /etc/systemd/system/forgejo.service https://codeberg.org/forgejo/forgejo/raw/branch/forgejo/contrib/systemd/forgejo.service
sudo systemctl daemon-reload
sudo systemctl enable forgejo.service
sudo systemctl start forgejo.service
```
#### Vérifier sur (http://localhost:3000/)

#### Choix des configurations sur la page d'entrée
<img width="948" height="758" alt="image" src="https://github.com/user-attachments/assets/14cba57c-ab65-4a54-bbec-174fda0246a2" />

#### Appuyer sur le boutons installer
<img width="1885" height="743" alt="image" src="https://github.com/user-attachments/assets/7a989c8b-bab8-4a6b-b95d-c28512e2b00e" />

#### Configuration supplémentaire dans l’app.ini de Forgejo

```bash
sudo systemctl stop forgejo.service
```

### Automatiser le processus d'installation
#### Script Bash a mettre dans un fichier installation.sh 
```bash
#script bash
# Installation
wget https://codeberg.org/forgejo/forgejo/releases/download/v13.0.2/forgejo-13.0.2-linux-amd64
chmod +x forgejo-13.0.2-linux-amd64

# Clé GPS
gpg --keyserver keys.openpgp.org --recv EB114F5E6C0DC2BCDD183550A4B61A2DC5923710
wget https://codeberg.org/forgejo/forgejo/releases/download/v13.0.2/forgejo-13.0.2-linux-amd64.asc
gpg --verify forgejo-13.0.2-linux-amd64.asc forgejo-13.0.2-linux-amd64

# Déplacer forgejo
sudo cp forgejo-13.0.2-linux-amd64 /usr/local/bin/forgejo
sudo chmod 755 /usr/local/bin/forgejo

sudo apt install git git-lfs

sudo adduser --system --shell /bin/bash --gecos 'Git Version Control' \
  --group --disabled-password --home /home/git git
  
# Dossier pour les accès
sudo mkdir /var/lib/forgejo
sudo chown git:git /var/lib/forgejo && sudo chmod 750 /var/lib/forgejo

# Dossier pour app.ini
sudo mkdir /etc/forgejo
sudo chown root:git /etc/forgejo && sudo chmod 770 /etc/forgejo

# Installation du systemd
sudo wget -O /etc/systemd/system/forgejo.service https://codeberg.org/forgejo/forgejo/raw/branch/forgejo/contrib/systemd/forgejo.service

sudo systemctl daemon-reload

sudo systemctl enable forgejo.service
sudo systemctl start forgejo.service

# Ouvrir dans le navigateur : http://localhost:3000/
```  
Attention a ne pas oublier d'augmenter les droits du fichiers
```bash
chmod +x ./installation.sh
```

Puis exécutez le fichier d'installation (exemple ici : installation.sh) de la manière suivante :  
  
```bash
./installation.sh
```  

Enfin, accédez à Forgejo web en ouvrant http://localhost:3000/ dans le moteur de recherche.


# 2/ Backup
## Objectifs : 
1. En utilisant le logiciel restic , créer un script qui archive les données importante du service  
2. Configurez le logiciel cron pour qu’il exécute ce script toutes les heures  
3. En utilisant l’utilitaire rclone , transférez le backup sur un serveur distant (ex: Google Drive)

#### Restic
Voici le script d'installation de Restic sur Linux :
```bash
apt-get install restic
restic version
```


# 3/ Sécurité
1. Mettre en place les règles de pare-feux pour n’accepter que le traffic sur le port de votre service  
2. Configurez fail2ban pour que les tentatives de bruteforce (ex: login failed 5 fois de suite) soient repérées  
 De même, repérez l’énumération web (lorsqu’un attaquant essaie plein de pages au hasard)  

# 4/ Monitoring
Mettre en place un outil de monitoring de votre service (ex: Grafana), et exporter des métriques depuis le serveur vers ce service.  
Ce service de monitoring ne doit pas être exposé au réseau externe. On y accèdera par port forwarding en SSH.  

### Documentation utilisé 

su - : pour être sudo sans admin
restic : https://www.youtube.com/watch?v=5DjNjqLuLSs
