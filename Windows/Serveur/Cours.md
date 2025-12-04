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
WSUS fonctionne en HTTP mais il est possible de configurer un certificat SSL pour sécuriser les trafics entre vos serveurs WSUS et les machines clientes du dit serveur.  
  
Alternatives : Microsoft Intune (PAYANT mais nouveau WSUS lié à office 365)  
Micrrosoft Enndpoint Configuration manager  
Azure Update Management (que les serveurs)  
Windows Autopatch (lié à office 365)  
  
Licences : rien n'est gratuit, possèdent un support  

## GPO
Permet d'imposer des règles (de sécurités (pas de terminal), corporate (imposer un fond d'écran)) sur des machines à distance.
GPO de base (à la racine du domaine) : Default Domain Policy (si on ajoute une GPO à cette échelle elle sera affecté à tout le domaine)
GPO s'applique en décendant donc les fichiers en dessous ont cette GPO pas les fichiers au dessus. Les GPO peuvent être liés pour par exemple appliqués la même GPO mais à certains domaines.  
  
Groupe de sécurité : groupe dans l'AD spécifique à la sécurité  
  
Default Domain Policy : aller dans settings pour voir ce qu'il y a dedans (ce qui est relatif aux mots de passes - mots de passe relatifs interdit par la RGPD)  
  
Chiffrement : transformer une info pour qu'elle ne soit pas lisible.  
Hashage : destruction d'une information de manière unique.  
- Méthode de sécurisation du hashage - salage : ajoute des caractères à la fin du mot de passe pour avoir des hash différents.  
  
MDP sécurisé : changement de mots de passe réguliés (nul pousse à un MDP simple), multi-facteurs de vérification SMS-mail (hackable), vérification par une autre app (Google Authentificator), vérification matériel (badge).  

### Les conteneurs 
Les GPO sont reliées à un container de stratégie :  
- GPC (dans l'Active Directory) : Group Policy Container, sait où est stocker la GPO  
- GPT (dans le dossier SYSVOL de remoteworks.loc) : Group Policy Template, stock la GPO  
  
Fichier remoteworks.loc : un dossier par forêt avec des policies (là où sont stockés les GPO)  
GPO à différentes versions quand il est modifié. La version est accessible dans le GUID de la GPO (dans SYSVOL) sur le fichier GPT.  
Faire attention à ce que l'Active Directory et SYSVOL soit sur la même version OU ERREUR.

### Magasin central et fichiers ADM, ADMX, ADML
Permettent d'ajouter des GPO pour par exemple les vieux serveurs pour s'adapter aux ajouts du systèmes. Problèmes de retro-compatibilité. 

- ADM/ADMX : modèles d'administration
- ADML : fichier linguistiqueour les modèles d'administration

.exe = programme
.msi = package (contient un .exe + de quoi automatiser un .exe)

SSO (Single Sign On) : délègue la connection (exemple : se connecter à partir de gmail)  
Héritage : la GPO parente s'applique aux GPO enfants EN REVANCHE si GPO sur le même sujet la GPO enfant gagne. En revanche grâce à ENFORCED le GPO parents écrase le GPO enfant.  

### WMI
Filtres WMI permettent de faire le tri d'à qui s'applique la GPO ou quel groupe n'y a pas accès. Filtre d'abord par version d'OS puis permet de construire la requête WMI (type format SQL).  
Infound Rules : permet de modifier les règles de pare-feu.
On peut sauvegarder une GPO (recommandé de tout mettre au même endroit), pas nécessairement au même endrit que la machine. 
Fusion possible de deux GPO
GPO starter : template de départ pour les autres GPO si les GPO se mettent à jour pas nécessarire

On peut modifier les GPO SAUF celles dans Widows Settings
