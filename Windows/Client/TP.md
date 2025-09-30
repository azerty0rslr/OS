# TP 
## Jour 1 : déploiement et préparation
- Installer un poste Windows 10, 11 -- problème à rebouter un nouvel ISO sur VMware, nous avons donc récupéré une ancienne VM windows 11  
- Configurer les partitions correctement (UEFI/GPT, préparation pour BitLocker).  
<img width="663" height="161" alt="image" src="https://github.com/user-attachments/assets/b83c99bd-15d2-4a8b-85bb-2a90abcd465f" />  
<img width="1160" height="382" alt="image" src="https://github.com/user-attachments/assets/d6c39f1d-9eb8-43b6-a209-754988e957dc" />  
<img width="775" height="470" alt="image" src="https://github.com/user-attachments/assets/137ae358-95d6-41ff-80e2-724ca08958d4" />  
<img width="892" height="508" alt="image" src="https://github.com/user-attachments/assets/5d00b92d-0491-48f2-9c3e-e38b17d20a77" />  
<img width="611" height="472" alt="image" src="https://github.com/user-attachments/assets/f706da97-1d6d-4dd6-8e3b-c911d79b55bb" />  
<img width="623" height="486" alt="image" src="https://github.com/user-attachments/assets/864b0e65-6c12-41a2-888e-79fba4a3323e" />  


- Créer une image de référence avec Sysprep.
<img width="871" height="359" alt="image" src="https://github.com/user-attachments/assets/e423a517-f18f-4418-9fc8-140852f7cf64" />
<img width="380" height="291" alt="image" src="https://github.com/user-attachments/assets/6f760dbc-d513-4f8e-968e-f46a728e3947" />


- Déployer l’image sur au moins 2 postes via MDT ou WDS.
- Joindre les postes au domaine Active Directory.
- Créer des OU spécifiques (Stagiaires, IT, Direction).
- Affecter des GPO de base (mot de passe fort, verrouillage de session).
