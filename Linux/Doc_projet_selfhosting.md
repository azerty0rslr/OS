#  Projet selfhosting
Sur un serveur Debian 13 (trixie) nous devons installer un logiciel, nous avions choisi le logiciel Mealie qui est un répertoire de recettes de cuisine numérisées. Suite à des problèmes expliqués dans la doc, nous sommes passés sur le logiciel Ente, une plateforme de partage de photos cryptée de bout en bout (alternative à Google Photos, Apple Photos).
Projet réalisé par Manon ROUSSELIERE et Meven DESBOIS.  

# 1/ Installation du logiciel

## Jour 1 : 
### Mealie
La documentation nous donne uniquement un dockerfile que nous avons essayer de traduire. Voici les étapes que nous avons suivis :

#### 1/ Instalation des dépendances et clone du projet 
- Sur notre serveur Debian, nous avons commencé par installer les dépendances
    - python3 avec pip et venv
    - Node.JS
    - Curl
    - Postgressql
    - Yarn

- Clôner le projet : ``git clone https://github.com/mealie-recipes/mealie/``
  
#### 2/ Compiler le front-end de l'application
- Utilisation de yarn :
    - `yarn install`pour installer les dépendances
    - `yarn generate`pour lancer le script de compilation

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
C'est un fichier qui contient le code prêt a être compiler et les données associées

La compilation ne fonctionne pas car le dockerfile a plusieurs stagiaires qui créent des images différentes pour les différentes étapes de compilation et alléger l'application à la fin de la compilation. 
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

# create a binary for your platform
yarn build
```
Problème sur les VM, les espaces de stockage sont arrivés à saturation, nous avons donc dû migrer dans le labo et recommencer. 
Nouveau problème, en arrivant à l'étape yarn install nous avons les erreurs suivantes : Request "https://yarnpkg.com/latest-version" finished with status code 500, Request failed "500 Internal Server Error". Le serveur yarn est down. Nous devons donc attendre pour poursuivre notre TP.


1. Installer le logiciel sur le serveur, le compiler à partir des sources directement  
2. Valider le bon fonctionnement du logiciel et de toutes ses fonctionnalités  
3. Automatiser le processus d’installation  

# 2/ Backup

1. En utilisant le logiciel restic , créer un script qui archive les données importante du service  
2. Configurez le logiciel cron pour qu’il exécute ce script toutes les heures  
3. En utilisant l’utilitaire rclone , transférez le backup sur un serveur distant (ex: Google Drive)  

# 3/ Sécurité
1. Mettre en place les règles de pare-feux pour n’accepter que le traffic sur le port de votre service  
2. Configurez fail2ban pour que les tentatives de bruteforce (ex: login failed 5 fois de suite) soient repérées  
 De même, repérez l’énumération web (lorsqu’un attaquant essaie plein de pages au hasard)  

# 4/ Monitoring
Mettre en place un outil de monitoring de votre service (ex: Grafana), et exporter des métriques depuis le serveur vers ce service.  
Ce service de monitoring ne doit pas être exposé au réseau externe. On y accèdera par port forwarding en SSH.  

### Documentation utilisé 


su - : pour être sudo sans admin
