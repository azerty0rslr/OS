```pwsh
# 1/ Récupérer les arguments
# Ajouter 3 paramètres au script A FAIRE
# Boucle if pour vérifier le nombre d'arguments (doit être égal à 3) sinon erreur
if ( $args.Count -ne 3 ) {
	throw " Usage: $PSCommandPath -actix_log <actix_log> -nginx_access <nginx_access_log> -nginx_error
 <nginx_error_log>"
}


# 2/ Initialiser les variables
# Variable pour chaque argument qui l'associe à son chemin absolu
$ACTIX_LOG = Get-ChildItem -Path $args[0]
$NGINX_ACCESS_LOG = Get-ChildItem -Path $args[1]
$NGINX_ERROR_LOG = Get-ChildItem -Path $args[2]


# 3/ Les requêtes les plus fréquentes
# récupère les requête GET avec status 200 (pas l'un OU l'autre donc utilisation de SimpleMatch)
$STATUS = ($ACTIX_LOG | Select-String -Pattern "200 | GET" -SimpleMatch)

# parmis ces requêtes n'afficher QUE le chemin - on split par les espaces ("\s+" = suivi par au moins un espace)
$CHEM = ($STATUS | ForEach-Object{($_ -split "\s+")[7]})

# compter l'occurrence de chaques lignes uniques (Group-Object), trier par le nombre de fois où la ligne apparaît (Sort-Object Count -Descending)
$MOST_SERVED = $CHEM | Group-Object | Sort-Object Count -Descending
# L'AFFICHAGE
# $MOST_SERVED | Format-Table -AutoSize


# 4/ Filtrer les requêtes
# Liste d'extensions à retirer
$EXT = "png","css","ico","js"

# on prend chaque lignes qui ne correspondent pas aux extensions qu'on doit retirer (A LA FIN)
foreach ($e in $EXT) {
    $MOST_SERVED = $MOST_SERVED | Where-Object { $_.name -notmatch "\.$e$" }
}
# $MOST_SERVED | Format-Table -AutoSize


# 5/ Ne garder que les meilleurs
# prendre le nombre de requêtes > à 10 occurences
$PLUS_10 = $MOST_SERVED | Where-Object { $_.Count -gt 10 }

# Formatter la ligne en chemin : nombre A FAIRE
$MOST_SERVED = $PLUS_10 | ForEach-Object { "" }

# Mettre dans un fichier most_served.txt
$MOST_SERVED | Set-Content "most_served.txt"


# 6/ IP Blacklist
# stock de ceux qui correspondent
# AJOUTER CEUX QUI MANQUENT DEBUG, LOGIN, .GIT
$STOCK = ($NGINX_ACCESS_LOG | Select-String -Pattern "admin" -SimpleMatch)

# récupérer l'ip pour le mettre dans IP_BLACKLISTS
# récupère la ligne sans la partie juste avant A FAIRE
$IP_BLACKLISTS = $STOCK | ForEach-Object { 
	($_ -split "\s+")[1]
} 
Write-Host $IP_BLACKLISTS


# 7/ Encore plus de blacklist
# retire les lignes GET POST HEAD
$LISTE_REST = ($NGINX_ACCESS_LOG | Select-String -notmatch "GET")
$LISTE_REST = ($LISTE_REST | Select-String -notmatch "POST")
$LISTE_REST = ($LISTE_REST | Select-String -notmatch "HEAD")

# prendre les IP de ce qui reste (donc plus de GET, POST, HEAD)
$IP_REST = ($LISTE_REST | ForEach-Object{($_ -split "\s+")[0]})

# ajouter à IP_BLACKLISTS
#$IP_BLACKLISTS += $IP_REST
 
# Trier les IP, retirer tous les doublons
#$IP_BLACKLISTS = $IP_BLACKLISTS | Sort-Object -Unique

# Mettre $IP_BLACKLISTS dans un fichier ip_blacklist.txt
#$IP_BLACKLISTS | Set-Content "ip_blacklist.txt"


# 8/ Repérer les downtimes
# Stock les erreurs du type donné A FAIRE
$DOWNTIME = "connect\(\) failed \(111: Unknown error\) while connecting to upstream"

# Récupère la date et l'heure de ce qui reste
$DOWNTIME | ForEach-Object {
        $date = ($_ -split "\s+")[0]
        $time = ($_ -split "\s+")[1]
	# Mettre les résultats au bon format date heure DOWN
        "$date $time DOWN"
    }
Write-Host $DOWNTIME


# 9/ Récupérer les uptimes
```
