WDS : Windows Deployment Services - pour l'automatisation d'installations. Prends une image à distance sur PXE ou WDS et 
boot l'OS sur la machine (des fois même préconfiguré). Aujourd'hui WDS est déprécié donc ne marche plus à partir de windows 11 donc windows 10 mais déprécié. 
Fonctionne avec des images d'installation (.wim). 

## DNS
Dans le DNS : 
- zone de recherche directe : donner un nom et recevoir son IP
- zone de recherche inversée : interroger le serveur DNS avec une IP pour avoir un nom de domaine

Full Qualified Domaine Name : le nom de domaine avec un . à la fin (google.com.)  
On achète (location) les noms de domaines à l'Afnic, qui utilise des sous-traitants (gandi, OVH ...) ayant des DNS.  
who.is récupère les infos liées au nom de domaine.  
DNS menteurs : permets de bloquer des sites  

### Type d'enregistrements DNS
Un nom de domaine ne vaut pas nécessairement UNE ip.
- A : il s’agit des enregistrements d’adresses faisant correspondre un nom d’hôte à une adresse IPv4 de 32bits. En IPv6, on utilise des enregistrements AAAA codés sur 128bits.  
- CNAME : il s’agit d’enregistrements canoniques créant un alias d’un domaine vers un autre. L’alias hérite de tous les sous-domaines de l’original.  
- MX : définit les serveurs de messagerie pour le domaine.  
- PTR : associe une adresse IP à un enregistrement de nom de domaine (on parle de reverse puisqu’il s’agit du contraire de l’enregistrement A).  
- NS : définit les serveurs DNS du domaine (primaire et secondaire).  
- SOA : fournit les informations générales de la zone : serveur principal, contact, délai d’expiration, n° de série de la zone.  
- SRV : généralise la notion d’enregistrement MX en proposant des fonctions avancées : taux de répartition de charge (décrit dans la RFC2782).  
- NAPTR : donne accès aux règles de réécriture de l’information permettant de lier  le nom de domaine et une ressource (RFC3403).  
- TXT : permet à l’administrateur d’insérer un texte quelconque pour un enregistrement DNS.

## WSUS
On peut choisir quelles machines sont affectées par les maj (groupes, individuellement, totalité). Si MAJ importante 1.18 à 1.19, voir toujours pour l'aval des personnes consernées. On peut créer des GPO (pour par exemple installer des MAJ en local, interdire de faire des MAJ depuis internet).  
Pour déployer MAJ sur grosse entreprise d'abord le faire sur une machine test pour voir si tout les logiciels démarres encore.  
Possible d'ajouter des machines à WSUS même si pas sur le domaine grâce aux GPO (GPO : modification de la base de registre).  

