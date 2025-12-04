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
  
# Mission 2 — Recherche & sélection de GPO pertinentes
Vous devez proposer au moins 10 GPO avancées, réparties en :  
  
## Sécurité du poste
Exemples possibles :  
- Désactivation de PowerShell pour les utilisateurs standard  
- Restriction d’accès au panneau de configuration  
- Désactivation du stockage USB  
- Mise en place d’un écran de veille verrouillé obligatoire  
- Mise en place de GPO dédié a Firefox **(obligatoire celle la!)**  

## Corporatisme & identité visuelle
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
