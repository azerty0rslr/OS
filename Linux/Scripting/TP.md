```bash
# 1/ Condition 3 arguments renseignés

# si le nombre d'argument renseigné est différent de 3 - erreur
if [[ "$#" -ne 3 ]]; then
	echo "Usage : $0 <actix_log> <nginx_access_log> <nginx_error_log>"; 
	exit 1; 
fi


# 2/ Initialiser une variable avec le chemin absolu du fichier

# on a donc les 3 arguments, on récupère leurs chemins absolus (realpath -s pour ne pas avoir les liens symboliques)
ACTIX_LOG=$(realpath -s $1)
NGINX_ACCESS_LOG=$(realpath -s $2)
NGINX_ERROR_LOG=$(realpath -s $3)
echo $ACTIX_LOG $NGINX_ACCESS_LOG $NGINX_ERROR_LOG


# 3/ ACTIX_LOG et les requêtes
# récupère les requête GET avec status 200 (grep)
REQ=$(grep "200 | GET" $ACTIX_LOG)

# parmis ces requêtes n'afficher QUE le chemin (awk)
CHEM=$(echo "$REQ" | awk '{print $7}')

# tri par ordre alphabétique (sort), ne garder que les lignes uniques et les compter (uniq -c), trier par le nombre de fois où la ligne apparaît (ordre decroissant : sort -nr)
MOST_SERVED=$(echo "$CHEM" | sort | uniq -c | sort -nr)


# 4/ MOST_SERVED et les extensions
# écraser MOST_SERVED avec une boucle pour retirer les lignes avec les extensions de la liste (grep -v) (PAS QUI CONTIENNENT CE MOT donc \.$ à la fin pour stipuler fin de ligne)
LIST=("png" "css" "ico" "js")
for ext in "${LIST[@]}"; do
	MOST_SERVED=$(echo "$MOST_SERVED" | grep -v "\.${ext}$")
done


# 5/ Requêtes suppérieures à 10
> most_served.txt

# read -r pour lire MOST_SERVED SANS interpréter les /
echo "$MOST_SERVED" | while read -r nombre chemin ; do
	# condition > 10 apparission
	if [[ "$nombre" -gt 10 ]]; then
		# on mets le résultat à part dans le bon format
		echo "$chemin : $nombre" >> most_served.txt
	fi
	
done


# 6/ Blacklister des IP du fichier NGINX_ACCESS_LOG
> ip_blacklist.txt

# ceux qu'il faut trouver et blacklist
LIST_INTERDITE=("admin" "debug" "login" ".git")

for i in "${LIST_INTERDITE[@]}"; do
	# stock de ceux qui correspondent
	STOCK=$(grep "$i" "$NGINX_ACCESS_LOG")
	# récupérer l'ip (en 1) pour le stocker dans le fichier
	IP=$(echo "$STOCK" | awk '{print $1}')
	# stocker dans fichier
	echo "$IP" >> ip_blacklist.txt
done


# 7/ Blacklister les méthodes différentes de GET, POST, HEAD
# retire les lignes GET POST HEAD
NGINX_ACCESS=$(grep -v "GET" "$NGINX_ACCESS_LOG")
NGINX_ACCESS=$(echo "$NGINX_ACCESS" | grep -v "POST")
NGINX_ACCESS=$(echo "$NGINX_ACCESS" | grep -v "HEAD")

# prendre les IP de ce qui reste (donc plus de GET, POST, HEAD)
IP_DIFF=$(echo "$NGINX_ACCESS" | awk '{print $1}')

# ajouter au fichier
echo "$IP_DIFF" >> ip_blacklist.txt

# trier et supprimer les doubles
sort ip_blacklist.txt > ip_tri.txt
uniq ip_tri.txt > ip_blacklist.txt


#8/ Repérer les downtimes
# faire grep pour trouver les erreurs nommées 
ERR=$(grep "connect() failed (111: Unknown error) while connecting to upstream" "$NGINX_ERROR_LOG") 
	
# récupérer la date et l'heure et les mettres au bon format (date heure DOWN) dans une variable DOWNTIME
DOWNTIME=$(echo "$ERR" | awk '{print $1 " " $2 " DOWN"}')


# 9/ Récupérer la date de ACTIX_LOG
# dans ACTIX_LOG récupérer la partie liée à la date
DATE=$(awk '{print $1}' "$ACTIX_LOG")

# grâce aux regex et à la commande sed -nr "s+<votre regex ici>.*+<format>+p"
DATE_REGEX="([0-9]{4})-([0-9]{2})-([0-9]{2})"
TIME_REGEX="([0-9]{2}):([0-9]{2}):([0-9]{2})"

# transformer la date au format YYYY/MM/DD H:M:S UP et enregistrer dans variable UPTIME
# Pas le temps de finir je n'ai que la partie avec l'année
UPTIME=$(echo "$DATE" | sed -nr "s+$DATE_REGEX.*+/\1/\2/\3/+p")
echo $UPTIME
```
