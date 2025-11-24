# Bash et powershell
## Bash
Monde académique, open-source  
Bourne Again Shell (shell = interface avec l'OS) - philosophie UNIX : faire une seule chose, le faire bien.  
UNIX (Ken Thomson) : création de BSD (MacOS) et Linux.  
Bash : interfacce textuelle, utilisateur ou script, gestion des commandes lancées.  
  
Bash : on entre une commande, split la commande et l'argument, cherche la coommande puis l'exécute, gère le processus puis fin.  
(Administration serveur, services récurrents, devops, automatisation, adaptiation, IaC)  
Suit le standard POSIX. Exécute des commandes dans un environment, peut exécuter des scripts de commandes. Beaucoup d'outils CLI.  

Bash assignation de variables SANS ESPACE (var=1), si besoin de coller deux variables (${var_1}bonjours)
export var : valeur dispo pour tout le processus (garder la variable)
Remplacer une variable - valeur = "patate $VAR"
Séparer les commandes par un ; si sur la même ligne
grep : remplacement

Condition : ne test que l'exécution de la commande (erreur ou non)
- e : existe
- 

## Powershell
Open source depuis 2016 (créé en 2006) basé sur framework .NET, cmdlets sont les commandes de powershell ce sont des fonctions de powershell (pas des logiciels comme sur bash). 
Script orienté objet, commandes retournent un objet, objets ont des attributs associés (éviter le parsing)
