# TP 
serveur mdp : admin2025.
## Jour 1 : déploiement et préparation
### - Installer un poste Windows 10, 11 
  
Nous avons utilisé un iso de Windows 11.  
  
### - Configurer les partitions correctement (UEFI/GPT, préparation pour BitLocker).
  
Sur la configuration de la VM nous avons bien activé EFI. Dans informations système, vérifier que Mode BIOS est bien en UEFI.  
  
<img width="1160" height="382" alt="image" src="https://github.com/user-attachments/assets/d6c39f1d-9eb8-43b6-a209-754988e957dc" />  
  
Pour vérifier que GPT est bien activé sur les disques : dans DISKPART entrer list disk et vérifier que GPT est bien en *.  
  
<img width="663" height="161" alt="image" src="https://github.com/user-attachments/assets/b83c99bd-15d2-4a8b-85bb-2a90abcd465f" />  
  
  
### - Créer une image de référence avec Sysprep.
  
Dans C:\Windows\System32\Sysprep sélectionner  sysprep.exe puis pour créer l’image de référence sélectionner « Généraliser » et mettre « Arrêter le système ».  
  
<img width="871" height="359" alt="image" src="https://github.com/user-attachments/assets/e423a517-f18f-4418-9fc8-140852f7cf64" />  
<img width="380" height="291" alt="image" src="https://github.com/user-attachments/assets/6f760dbc-d513-4f8e-968e-f46a728e3947" />  
  
  
### - Déployer l’image sur au moins 2 postes via MDT ou WDS.  
```
New-Item -Path "D:\DeploymentShare" -ItemType Directory
```
#### Créé le partage 
```
New-SmbShare -Name "DeploymentShare$" -Path "D:\DeploymentShare" -FullAccess "Administrators"
```

#### Importer l'image Windows dans MDT 
```
Import-MDTOperatingSystem -Path "D:\DeploymentShare\Operating Systems" `
  -SourcePath "D:\Sources\Windows11" `
  -DestinationFolder "Win11"

```
Étape qui ne peut pas se faire via Powershell, ouvrir le dépoyment Workbench:  
DeploymentShare > Update Deployment Share> LiteTouchEP_x643.win  
  
Ajouter l'image LiteTouchPE au serveur  

```
wdsutil /Add-Image /ImageType:Boot /ImageFile:"D:\DeploymentShare\Boot\LiteTouchPE_x64.wim" /ImageName:"LiteTouch PE x64"
````

  
### - Joindre les postes au domaine Active Directory.  
Joindre le nom de domaine  
```
netdom join %Cam% /domain:camelia.local /userd:Administrateur /passwordd:Admin1234
```
Redémarrer le poste
```
shutdown /r /t 0
```

  
### - Créer des OU spécifiques (Stagiaires, IT, Direction).  
```
dsadd ou "OU=Stagiaires,DC=camelia,DC=local"
```
GPO étape manuel  
Créer et lier la GPO  
Apporter les modifications  
Configuration ordinateur -> Paramètres Windows -> Paramètres de sécurité  
  
Pour un mot de passe plus fort aller dans  Stratégies de compte -> Stratégie de mot de passe  
Verouiller session après inactivité Configuration utilisateur -> Modèles d'administration -> Composants Windows → Session -> Délai d’inactivité  

  
### - Affecter des GPO de base (mot de passe fort, verrouillage de session).  
  
## Jour 2 : Sécurisation et GPO avancées
### - Activer bitlocker  
  
Dans « Panneau de configuration » sélectionner « Chiffrement de lecteur BitLocker » puis sélectionner « Activer BitLocker », on récupère la clé de récupération BitLocker.  
  
<img width="651" height="362" alt="image" src="https://github.com/user-attachments/assets/bb4adb48-24b2-40f5-a236-c9fdf90803e9" />  
<img width="775" height="470" alt="image" src="https://github.com/user-attachments/assets/137ae358-95d6-41ff-80e2-724ca08958d4" />  
<img width="892" height="508" alt="image" src="https://github.com/user-attachments/assets/5d00b92d-0491-48f2-9c3e-e38b17d20a77" />

Ensuite je sélectionne « Ne chiffrer que l’espace disque utilisé » puis « Nouveau mode de chiffrement ». On exécute la vérification du système BitLocker.  
  
<img width="627" height="495" alt="image" src="https://github.com/user-attachments/assets/3352707f-128c-4c58-b77d-a35a7c50dfce" />  
<img width="611" height="472" alt="image" src="https://github.com/user-attachments/assets/f706da97-1d6d-4dd6-8e3b-c911d79b55bb" />  
<img width="623" height="486" alt="image" src="https://github.com/user-attachments/assets/864b0e65-6c12-41a2-888e-79fba4a3323e" />  
  
### - Activer Credential Guard et Virtualization-Based Security (VBS)  
  
Dans « Editeur de stratégie de groupe locale » je vais dans Configuration ordinateur -> Modèles d’administration -> Système -> Device Guard.  
  
<img width="762" height="541" alt="image" src="https://github.com/user-attachments/assets/c0bd572b-259a-4b85-b3bc-cd92fe49411c" />
  
Sélectionner “Activer la sécurité basée sur la virtualization” puis dans “Configuration Credential Guard” mettre Activé.  
  
<img width="775" height="542" alt="image" src="https://github.com/user-attachments/assets/1629fc31-f563-40f4-82b9-7fd461170210" />  
<img width="698" height="631" alt="image" src="https://github.com/user-attachments/assets/c8897429-18d5-445e-9678-6b9a98193914" />

Dans « Information système » vérifier que « Sécurité basée sur la virtualisation » est activé et que « Services configurés pour la sécurité » est bien sur Credential Guard.  
  
<img width="1170" height="581" alt="image" src="https://github.com/user-attachments/assets/d0a4f0da-6826-4c82-8f73-0b760926b05d" />
<img width="1177" height="570" alt="image" src="https://github.com/user-attachments/assets/9d03236f-3e98-4cb2-8dd9-518de6c2a59e" />  

### - Configurer Windows Defender avec exclusions et analyse planifiée  
  
Dans « Sécurité Windows » sélectionner « Protection contre les virus et menaces » puis « Paramètres de protection contre les virus et menaces ».  
  
<img width="803" height="633" alt="image" src="https://github.com/user-attachments/assets/dc8200c3-75cd-47b7-9a4f-5b927e26736b" />  
<img width="805" height="635" alt="image" src="https://github.com/user-attachments/assets/fe019683-2ac6-496e-8313-1b880092fb0d" />  
<img width="722" height="635" alt="image" src="https://github.com/user-attachments/assets/56810809-09db-4401-b393-a9686a86d10e" />
  
| _Créer analyse planifiée_  
Aller dans « Planificateur de tâches » puis dans Bibliothèque du Planificateur -> Microsoft -> Windows -> Windows Defender -> Windows Defender Scheduled Scan -> Déclencheurs. Puis dans nouveau créer un nouveau déclencheur pour créer une analyse planifiée au moment désiré.  
  
<img width="795" height="573" alt="image" src="https://github.com/user-attachments/assets/6193e82c-a280-4f48-a9bf-408dd22f40fa" />  
<img width="787" height="567" alt="image" src="https://github.com/user-attachments/assets/118fdf9f-f633-4d8f-ba30-fdaa51cf057c" />  
<img width="635" height="491" alt="image" src="https://github.com/user-attachments/assets/7c8cddac-02d4-4c3e-931f-d6f37ca2efa7" />  
<img width="592" height="518" alt="image" src="https://github.com/user-attachments/assets/49994ce5-ad57-4108-a6f2-5f2f1db7019b" />
  
| _Ajouter des exclusions_  
Dans « Sécurité Windows » sélectionner « Protection contre les virus et menaces » puis descendre jusqu’à « Exclusions » et faire « Ajouter ou supprimer des exclusions » puis ajouter une exclusion sur un Fichier/Dossier/Type de fichier/Processus (ici j’ai mis une exclusion sur le dossier Temp).  
  
<img width="759" height="652" alt="image" src="https://github.com/user-attachments/assets/1c7850c3-056a-4ba5-8827-85d11077c0c9" />  
<img width="808" height="644" alt="image" src="https://github.com/user-attachments/assets/806f1043-5df9-45bf-8321-cdbcea1cea12" />  
<img width="815" height="636" alt="image" src="https://github.com/user-attachments/assets/5e97170b-19ad-4e98-8e33-de5775099c85" />  
<img width="493" height="478" alt="image" src="https://github.com/user-attachments/assets/34ccc0eb-94ac-42fe-9329-8fe6c1d8bba9" />  
<img width="462" height="362" alt="image" src="https://github.com/user-attachments/assets/a7a2d130-bc6c-4518-b24d-ea02981d2df4" />  


### - Déployer et tester des GPO supplémentaires :  
  * Redirection de dossiers utilisateurs (Documents, Bureau) vers un partage réseau.  
  * Déploiement d’un script de connexion/déconnexion.  
  * Restriction d’accès au Panneau de configuration et aux paramètres Windows.  
  * Mise en place d’AppLocker avec règles différenciées (Stagiaires vs IT).  
  * Interdiction des périphériques USB sauf pour l’équipe IT.  
  * Déploiement de préférences GPO (lecteurs réseaux, imprimantes par défaut, fond d’écran de l’entreprise).  

 ## Jour 3 : Administration distante, mise à jour et dépannage
### - Configurer et tester le Bureau à distance (RDP sécurisé avec NLA).  

Dans les paramètres, aller dans « Système » puis « Bureau à distance ». Activer « Bureau à distance ».  
  
<img width="817" height="642" alt="image" src="https://github.com/user-attachments/assets/bb5de989-17b5-43cb-872a-a806907fcb7a" />  
<img width="747" height="357" alt="image" src="https://github.com/user-attachments/assets/6445811c-90f9-4528-9f58-9e58aa8266f9" />
<img width="812" height="642" alt="image" src="https://github.com/user-attachments/assets/634c694c-8d2f-47aa-9f9f-a28a984bcada" />  

Depuis mon PC hôte, je peux désormais mettre l'adresse IP de la VM sur "Connexion Bureau à distance" et me connecter à la VM.  
  
 <img width="562" height="313" alt="image" src="https://github.com/user-attachments/assets/fc4062a7-2de0-4142-90d4-0622b8d103dd" />  

### - Créer et tester un accès VPN vers le réseau pédagogique.  
### - Mettre en place un serveur WSUS, approuver des mises à jour et forcer leur application via GPO.  
### - Vérifier et personnaliser les règles du pare-feu Windows Defender (in/out).  

| _Autoriser trafic entrant depuis le port TCP 80_  
Dans « Pare-feu Windows Defender avec fonctions avancées de sécurité » puis dans « Règles de trafic entrant » faire « Nouvelle règle de trafic entrant » et créer la règle de trafic (ici sur le Port, TCP 80 qu’on autorise sur le domaine, privé et public).  
Même principe pour les Règles de trafic sortant.  
  
<img width="1060" height="586" alt="image" src="https://github.com/user-attachments/assets/fd83b1c4-56e9-43ef-bf33-107cd0488e8f" />  
<img width="747" height="477" alt="image" src="https://github.com/user-attachments/assets/702ecf2e-9633-458a-b85f-8cc5c3220a83" />  
<img width="728" height="583" alt="image" src="https://github.com/user-attachments/assets/01efcb86-a545-4e8a-92f5-22f446777a3c" />  
<img width="736" height="602" alt="image" src="https://github.com/user-attachments/assets/7cec3b5f-fdff-437a-9fbd-76dbbc9f3370" />  
<img width="726" height="587" alt="image" src="https://github.com/user-attachments/assets/ceb731e5-d46b-4d2d-ac2b-977896dd3aef" />  
<img width="737" height="602" alt="image" src="https://github.com/user-attachments/assets/b9d83935-b111-4474-b194-194c1f7ecad2" />  

### - Scénarios de dépannage :  
  * Poste qui ne démarre plus : réparer avec BCD, SFC, DISM ou WinRE.
  
Sur PowerShell (en administrateur), vérifier l’intégrité du système avec la commande sfc /scannow. Si nécessaire de réparer l’image Windows faire DISM /Online /Cleanup-Image /RestoreHealth.  
  
<img width="1020" height="347" alt="image" src="https://github.com/user-attachments/assets/8389a2d1-b736-434a-97af-a7fa5f7e139a" />  
<img width="867" height="180" alt="image" src="https://github.com/user-attachments/assets/bf44f757-2cce-42d2-8d4e-160c49417f20" />  
  
Réparer le BCD via WinRE si nécessaire avec les commandes suivantes :  
```cmd
bootrec /fixmbr  
bootrec /fixboot  
bootrec /rebuildbcd
```

  * Perte de profil utilisateur : recréer et restaurer les données.  

| _Recréer un profil (simulation de la perte du profil utilisateur Manon qui est remplacé par le profil TestUser)_  
Dans PowerShell en administrateur faire :  
```cmd
net user TestUser /add
net localgroup Administrateurs TestUser /add
```

<img width="997" height="530" alt="image" src="https://github.com/user-attachments/assets/b573c702-a378-407a-8933-9924b661f37e" />  

Puis dans C:\Utilisateurs, copier coller les données du profil perdu sur le nouveau profil.  
  
<img width="860" height="626" alt="image" src="https://github.com/user-attachments/assets/a276c363-c9e0-4d18-a462-a194b10a37cf" />  

  * GPO qui ne s’applique pas : analyse avec gpresult /h et Event Viewer.  
  * Lenteurs réseau : diagnostic avec Resmon, BranchCache et tests complémentaires.
  
Pour vérifier le fonctionnement du réseau en cas de lenteur, dans « Gestionnaire des tâches » pour voir la gestion de la performance de connexion (ici Ethernet) dans « resmon » /« Moniteur de ressources » pour voir la gestion des ressources par le PC.  
  
 <img width="782" height="592" alt="image" src="https://github.com/user-attachments/assets/c5378613-b533-400a-b018-b8a0f539ff37" />  
 <img width="795" height="598" alt="image" src="https://github.com/user-attachments/assets/10624a0a-7241-4a99-ba30-ec66ddccb7e8" />
  
Pour tester la connectivité : 2 commandes possibles dans le CMD « ping » et « tracert ».  
  
<img width="1000" height="533" alt="image" src="https://github.com/user-attachments/assets/2c270866-09a7-42dc-81d8-d37fe3efa7f5" />  
