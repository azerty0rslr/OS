# TP 
## Jour 1 : déploiement et préparation
- Installer un poste Windows 10, 11 -- problème à rebouter un nouvel ISO sur VMware, nous avons donc récupéré une ancienne VM windows 11  
- Configurer les partitions correctement (UEFI/GPT, préparation pour BitLocker).  
<img width="663" height="161" alt="image" src="https://github.com/user-attachments/assets/b83c99bd-15d2-4a8b-85bb-2a90abcd465f" />  
<img width="1160" height="382" alt="image" src="https://github.com/user-attachments/assets/d6c39f1d-9eb8-43b6-a209-754988e957dc" />  
 

- Créer une image de référence avec Sysprep.
<img width="871" height="359" alt="image" src="https://github.com/user-attachments/assets/e423a517-f18f-4418-9fc8-140852f7cf64" />
<img width="380" height="291" alt="image" src="https://github.com/user-attachments/assets/6f760dbc-d513-4f8e-968e-f46a728e3947" />

- Déployer l’image sur au moins 2 postes via MDT ou WDS.
- Joindre les postes au domaine Active Directory.
- Créer des OU spécifiques (Stagiaires, IT, Direction).
- Affecter des GPO de base (mot de passe fort, verrouillage de session).

## Jour 2 : Sécurisation et GPO avancées
- Activer bitlocker
<img width="775" height="470" alt="image" src="https://github.com/user-attachments/assets/137ae358-95d6-41ff-80e2-724ca08958d4" />
<img width="627" height="495" alt="image" src="https://github.com/user-attachments/assets/3352707f-128c-4c58-b77d-a35a7c50dfce" />
<img width="892" height="508" alt="image" src="https://github.com/user-attachments/assets/5d00b92d-0491-48f2-9c3e-e38b17d20a77" />  
<img width="611" height="472" alt="image" src="https://github.com/user-attachments/assets/f706da97-1d6d-4dd6-8e3b-c911d79b55bb" />  
<img width="623" height="486" alt="image" src="https://github.com/user-attachments/assets/864b0e65-6c12-41a2-888e-79fba4a3323e" />

- Activer Credential Guard et Virtualization-Based Security (VBS)
- Configurer Windows Defender avec exclusions et analyse planifiée
- Déployer et tester des GPO supplémentaires :  
  * Redirection de dossiers utilisateurs (Documents, Bureau) vers un partage réseau.  
  * Déploiement d’un script de connexion/déconnexion.  
  * Restriction d’accès au Panneau de configuration et aux paramètres Windows.  
  * Mise en place d’AppLocker avec règles différenciées (Stagiaires vs IT).  
  * Interdiction des périphériques USB sauf pour l’équipe IT.  
  * Déploiement de préférences GPO (lecteurs réseaux, imprimantes par défaut, fond d’écran de l’entreprise).  

 ## Jour 3 : Administration distante, mise à jour et dépannage
 - Configurer et tester le Bureau à distance (RDP sécurisé avec NLA).  
- Créer et tester un accès VPN vers le réseau pédagogique.  
- Mettre en place un serveur WSUS, approuver des mises à jour et forcer leur application via GPO.  
- Vérifier et personnaliser les règles du pare-feu Windows Defender (in/out).  
- Scénarios de dépannage :  
  * Poste qui ne démarre plus : réparer avec BCD, SFC, DISM ou WinRE.  
  * Perte de profil utilisateur : recréer et restaurer les données.  
  * GPO qui ne s’applique pas : analyse avec gpresult /h et Event Viewer.  
  * Lenteurs réseau : diagnostic avec Resmon, BranchCache et tests complémentaires.  
