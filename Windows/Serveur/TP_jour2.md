# Contexte général :
Vous êtes nouvellement recruté(e) en tant qu’alternant administrateur/trice systèmes dans l’entreprise ACME Solutions, une ESN de 100 employés présente 
sur 3 sites (Lyon, Paris, Lille). Le DSI souhaite renforcer :  
- la sécurité des postes clients  
- l’homogénéité du parc informatique  
- la conformité vis-à-vis des normes internes  
- le contrôle des navigateurs, notamment Firefox  
- la réduction des incidents liés à une mauvaise configuration  
  
Le parc informatique utilise :  
- Active Directory Windows Server 2019-2022  
- 100 postes Windows 10/11  
- Firefox ESR comme navigateur standard  
  
Votre mission : proposer, justifier et déployer un ensemble cohérent de GPO avancées, puis fournir un POC (Proof of Concept) fonctionnel.  
  
# Mission 1 — Analyse des besoins & rédaction d’un mini-cahier des charges
Rédigez un document expliquant :  
1. Les risques actuels sur les postes (sécurité, configuration hétérogène, productivité).  
2. Vos objectifs techniques.  
3. Un plan de déploiement GPO haute-niveau (schéma d'OU, ordre de priorité, etc.)

### Mise en application
1.	Les risques actuels sur les postes (sécurité, configuration hétérogène, productivité).  
En prenant en compte la configuration actuelle de l’entreprise, les postes présentent différents risques. Tout d’abord, il y a différentes versions dans l’entreprise de Windows (Windows 10 et Windows 11 – il est important de prendre en compte qu’il n’y a plus de support sur Windows 10 depuis octobre 2025 donc plus de mises à jour de sécurité) il y a également deux versions de Active Directory Windows Server (2019 et 2022 – Active Directory Windows Server 2022 est plus poussé en termes de sécurité et de cloud).  
  
2.	Vos objectifs techniques.  
Le DSI souhaitant renforcer la sécurité des postes clients ainsi que l’homogénéité du parc informatique, le premier objectif technique est de passer les postes encore sur Windows 10 sur Windows 11. Le second objectif serait donc de passer sur Active Directory Windows Server 2022. Pour ces deux objectifs, il est important de d’abord obtenir l’aval des personnes utilisant ces postes pour être sûr qu’aucun logiciel ne soit impacté. Afin de renforcer le contrôle des navigateurs (ici Firefox ESR), nous appliquerons des GPO pour Firefox. Enfin dans le cadre du renfort de la conformité vis-à-vis des normes internes ainsi que de la réduction des incidents liés à une mauvaise configuration nous séparerons les postes en trois groupes (séparation géographique entre Lyon, Paris et Lille) auxquelles nous affecterons différentes GPO (interdiction d’utiliser Powershell, désactivation du stockage USB …).  
  
3.	Un plan de déploiement GPO haute-niveau (schéma d'OU, ordre de priorité, etc.)  
Pour le schéma d’OU (unités organisationnelles), les postes seront divisés en 4 groupes qui auront l’ordre de priorité suivant : réseaux puis développement et administration puis commerciaux. Ces groupes seront eux-mêmes divisés géographiquement : Lyon, Paris et Lille.
  
    
# Mission 2 — Recherche & sélection de GPO pertinentes
Vous devez proposer au moins 10 GPO avancées, réparties en :  
  
## Sécurité du poste
Exemples possibles :  
- Désactivation de PowerShell pour les utilisateurs standard
- Restriction d’accès au panneau de configuration
- Désactivation du stockage USB
- Mise en place d’un écran de veille verrouillé obligatoire
- Mise en place de GPO dédié a Firefox (obligatoire celle la!)

## Corporatisme & identité visuelle
- Fond d’écran imposé selon le site  
- Messages légaux au login  
- Configuration du menu Start / barre des tâches  
- Désactivation Microsoft Store  
- Déploiement automatique de logiciels via GPO (MSI)

### Mise en application
#### Création des OU
Aller dans Outils -> Utilisateurs et ordinateurs Active Directory :  
<img width="761" height="535" alt="image" src="https://github.com/user-attachments/assets/086cca28-8f06-4e68-823f-2f0ddcec1136" />  
Créer les nouveaux OU :  
<img width="758" height="536" alt="image" src="https://github.com/user-attachments/assets/db5cb6e8-de18-49b5-9a2f-c7a10ce81bb8" />  
Puis créer toutes les divisions nécessaires :  
<img width="758" height="535" alt="image" src="https://github.com/user-attachments/assets/96e1202d-bf59-4a51-ab74-d77c23747332" />  

#### Mise en place des GPO
Pour créer une GPO allez tout d'abord dans Outils -> Gestion de stratégie de groupe  
<img width="758" height="537" alt="image" src="https://github.com/user-attachments/assets/e1c852fb-52dc-418f-8514-4393d0fee03d" />  
  
**Sécurité du poste :**
- Désactivation de PowerShell pour les utilisateurs standard  
<img width="757" height="540" alt="image" src="https://github.com/user-attachments/assets/05fdd54e-d0ca-4970-9907-2b03dc50c138" />  
Faire "Modifier" sur la GPO  
<img width="385" height="521" alt="image" src="https://github.com/user-attachments/assets/b9c18074-50e4-4132-bb76-eb2122c798bd" />  
Puis aller dans Configuration utilisateur -> Stratégies -> Paramètres Windows -> Paramètres de sécurité
-> Stratégies de restriction logicielle et faites "Nouvelles stratégie de restriction logicielle"  
<img width="797" height="455" alt="image" src="https://github.com/user-attachments/assets/6aa71e4d-a74d-4251-9ffe-364c63abb57f" />  
Faire clic droit sur Règles supplémentaires et cliquer sur "Nouvelle règle de chemin d'accès"  
<img width="542" height="605" alt="image" src="https://github.com/user-attachments/assets/29a059cc-93f1-4090-ad40-bebd22df660d" />  
Ajouter la nouvelle règle de chemin d'accès n'autorisant pas Powershell (faire Appliquer puis Ok)  
<img width="415" height="467" alt="image" src="https://github.com/user-attachments/assets/1125bb75-362a-401a-94c2-3d8e43a98a22" />  
Afin d'assurer une protection efficace, il faut bloquer l'ensemble des exécutables Powershell suivant, sinon la configuration sera incomplète :  
<img width="580" height="86" alt="image" src="https://github.com/user-attachments/assets/59974b35-3baa-4b89-9d45-ccccc849e4b3" />
(Si il existe un fichier PowerShell 7 - 64 bits le bloquer aussi).  


- Restriction d’accès au panneau de configuration  
- Désactivation du stockage USB  
- Mise en place d’un écran de veille verrouillé obligatoire  
- Mise en place de GPO dédié a Firefox (obligatoire celle la!)  

**Corporatisme et identité visuelle :**
- Fond d’écran imposé selon le site  
- Messages légaux au login  
- Configuration du menu Start / barre des tâches  
- Désactivation Microsoft Store  
- Déploiement automatique de logiciels via GPO (MSI)  

  
# Mission 3 — Déploiement concret du POC
Sur un environnement Windows Server + client Windows :  
1. Créez une OU POC-GPO  
2. Ajoutez un poste et un utilisateur de test  
3. Créez et appliquez 10 GPO minimum parmi votre sélection  
4. Vérifiez leur application via : ```gpupdate /force```  ```gpresult /h rapport.html```  
5. Documentez vos observations (captures obligatoires)  
  
# Livrable
Rédaction d'un document explicitant votre travail sur les 3 missions.  
Pour la mission 3 il faudra fournir une documentation de la mise en place (screenshot et commentaire).  
Fournissez également une sauvegarde des GPO que vous avez mis en place  
