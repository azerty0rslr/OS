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
1.	Les risques actuels sur les postes (sécurité, configuration hétérogène, productivité).  
En prenant en compte la configuration actuelle de l’entreprise, les postes présentent différents risques. Tout d’abord, il y a différentes versions dans l’entreprise de Windows (Windows 10 et Windows 11 – il est important de prendre en compte qu’il n’y a plus de support sur Windows 10 depuis octobre 2025 donc plus de mises à jour de sécurité) il y a également deux versions de Active Directory Windows Server (2019 et 2022 – Active Directory Windows Server 2022 est plus poussé en termes de sécurité et de cloud).  
  
2.	Vos objectifs techniques.  
Le DSI souhaitant renforcer la sécurité des postes clients ainsi que l’homogénéité du parc informatique, le premier objectif technique est de passer les postes encore sur Windows 10 sur Windows 11. Le second objectif serait donc de passer sur Active Directory Windows Server 2022. Pour ces deux objectifs, il est important de d’abord obtenir l’aval des personnes utilisant ces postes pour être sûr qu’aucun logiciel ne soit impacté. Afin de renforcer le contrôle des navigateurs (ici Firefox ESR), nous appliquerons des GPO pour Firefox. Enfin dans le cadre du renfort de la conformité vis-à-vis des normes internes ainsi que de la réduction des incidents liés à une mauvaise configuration nous séparerons les postes en trois groupes (séparation géographique entre Lyon, Paris et Lille) auxquelles nous affecterons différentes GPO (interdiction d’utiliser Powershell, désactivation du stockage USB …).  
  
3.	Un plan de déploiement GPO haute-niveau (schéma d'OU, ordre de priorité, etc.)  
Pour le schéma d’OU (unités organisationnelles), les postes seront divisés en 4 groupes qui auront l’ordre de priorité suivant : réseaux puis développement et administration puis commerciaux. Ces groupes seront eux-mêmes divisés géographiquement : Lyon, Paris et Lille.  
  
    
# Mission 2 — Recherche & sélection de GPO pertinentes
## Création des OU
Aller dans Outils -> Utilisateurs et ordinateurs Active Directory :  
<img width="761" height="535" alt="image" src="https://github.com/user-attachments/assets/086cca28-8f06-4e68-823f-2f0ddcec1136" />  
Créer les nouveaux OU :  
<img width="758" height="536" alt="image" src="https://github.com/user-attachments/assets/db5cb6e8-de18-49b5-9a2f-c7a10ce81bb8" />  
Puis créer toutes les divisions nécessaires :  
<img width="758" height="535" alt="image" src="https://github.com/user-attachments/assets/96e1202d-bf59-4a51-ab74-d77c23747332" />  
  
## Mise en place des GPO
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
PowerShell est désormais bloqué sur les 4 groupes sauf sur le serveur de gestion.  
  
- Restriction d’accès au panneau de configuration  
Créer une nouvelle GPO  
<img width="766" height="542" alt="image" src="https://github.com/user-attachments/assets/d673d3e5-5957-46ee-bb54-56b68fb6c1e7" />  
Faire "Modifier" sur la GPO, puis dans Configuration utilisateur > Stratégies > Modèles d'administration > Panneau de configuration cliquer sur Interdire l'accès au Panneau de configuration et l'application Paramètres du PC.  
<img width="800" height="573" alt="image" src="https://github.com/user-attachments/assets/2bc50d4d-10c0-4743-ad87-4ea3024888bf" />  
Double cliquer, sélectionner activer puis Ok.  
<img width="690" height="645" alt="image" src="https://github.com/user-attachments/assets/77317e90-f2c6-4084-8ba9-302379d73981" />  
Le panneau de configuration est désormais bloqué sur les 4 groupes sauf sur le serveur de gestion.  
  
- Désactivation du stockage USB  
Créer une nouvelle GPO  
<img width="762" height="405" alt="image" src="https://github.com/user-attachments/assets/a8c3bd3b-4def-4978-ad7b-a8fc68f5a439" />  
Faire "Modifier" sur la GPO, puis dans Configuration ordinateur > Stratégies > Modèles d'administration > Système > Accès au stockage amovible cliquer sur Disques amovibles : refuser l'accès en exécution  
<img width="802" height="592" alt="image" src="https://github.com/user-attachments/assets/b1c80eea-375f-4109-9eb0-5012be686729" />  
Double cliquer, sélectionner activer puis Ok.  
<img width="690" height="647" alt="image" src="https://github.com/user-attachments/assets/e46f0344-1b9e-44b9-a83b-6709dbc8d12f" />  
Répéter l'opération pour les deux autres règles relatives aux disques amovibles.  
<img width="583" height="128" alt="image" src="https://github.com/user-attachments/assets/c3e7ca19-1eef-47e6-a17d-a448083b8d87" />  
Le stockage USB est désormais bloqué sur les 4 groupes sauf sur le serveur de gestion.  
  
- Mise en place d’un écran de veille verrouillé obligatoire  
Créer une nouvelle GPO  
<img width="763" height="537" alt="image" src="https://github.com/user-attachments/assets/4c8e6058-8337-4ed0-8b1b-8ab0c729bc77" />  
Faire "Modifier" sur la GPO, puis dans Configuration ordinateur > Stratégies > Paramètres Windows > Paramètres de sécurité > Stratégies locales > Options de sécurité cliquer sur Ouverture de session interactive : limite d'inactivité de l'ordinateur  
<img width="805" height="590" alt="image" src="https://github.com/user-attachments/assets/c0842607-acc9-4d59-a5da-48728257979a" />  
Double cliquer, sélectionner "Définir ce paramètre de stratégie" puis entrer le temps de mise en veille voulu (ici 6000sec soit 10min) puis faire ok.  
<img width="428" height="522" alt="image" src="https://github.com/user-attachments/assets/f08e0c23-e0a8-4f9b-89c3-e0e6099eabbd" />  
Un écran de veille verrouillé apparaît donc désormais au bout de 10min d'inactivité sur les 4 groupes sauf sur le serveur de gestion.  
  
- Mise en place de GPO dédié a Firefox (obligatoire celle la!)  
  
**Corporatisme et identité visuelle :**
- Fond d’écran imposé selon le site  
Créer une GPO dans l'une des villes, nous la lierons aux autres du même nom plus tard  
<img width="772" height="392" alt="image" src="https://github.com/user-attachments/assets/7bf5f3f3-0a4b-4df9-818b-e55b96e5f5c2" />  
Télécharger et stocker l'image destiné au fond d'écran  
Faire "Modifier" sur la GPO, puis dans « Configuration ordinateur », « Préférences », « Paramètres Windows » et « Fichiers » cliquer sur Nouveau avec le clic droit  
<img width="802" height="591" alt="image" src="https://github.com/user-attachments/assets/57e93210-23a6-41f2-9f18-110a793980a0" />  
  
- Messages légaux au login  
Créer une GPO  
<img width="765" height="537" alt="image" src="https://github.com/user-attachments/assets/8888edc4-bd7c-4301-a22a-ec192ad35cb3" />  
Faire "Modifier" sur la GPO, puis dans « Configuration ordinateur », "Stratégie" « Paramètres Windows », « Paramètres de sécurité », « Stratégies locales » et « Options de sécurité »  
<img width="823" height="613" alt="image" src="https://github.com/user-attachments/assets/1dd6e04a-ecdb-4b90-91a9-4404e55bc82a" />  
Double cliquer sur Ouverture de session interactive : titre du message pour les utilisateurs essayant de se connecter et remplissez le titre de votre message comme il suit  
<img width="422" height="521" alt="image" src="https://github.com/user-attachments/assets/8d09060c-7bee-4d3a-b71c-68678574bc3f" />  
Faire OK puis double cliquer sur Ouverture de session interactive : contenu du message pour les utilisateurs essayant de se connecter et remplissez le contenu de votre message comme il suit et faire OK  
<img width="428" height="516" alt="image" src="https://github.com/user-attachments/assets/df8c43f6-34ef-4bed-bd8c-35a63b0cedf7" />  
Un message s'affiche désormais à la connexion sur les 4 groupes sauf sur le serveur de gestion.  
  
- Configuration de la barre des tâches  
Tout d'abord épingler au menu Démarrer les applications que l'on souhaite épingler à la barre des tâches  
<img width="767" height="542" alt="image" src="https://github.com/user-attachments/assets/a89d6074-4e0a-4eca-aa16-86bd276da998" />  
Exécuter la commande suivante afin d'exporter la disposition du menu démarrer  
```Export-StartLayout -Path "$env:userprofile\Desktop\W10-Apps.xml"```  
Dans ce fichier le modifier de la manière suivante (il peut y avoir une erreur si Firefox n'est pas installé)  
```
<?xml version="1.0" encoding="utf-8"?>
<LayoutModificationTemplate
    xmlns="http://schemas.microsoft.com/Start/2014/LayoutModification"
    xmlns:defaultlayout="http://schemas.microsoft.com/Start/2014/FullDefaultLayout"
    xmlns:start="http://schemas.microsoft.com/Start/2014/StartLayout"
    xmlns:taskbar="http://schemas.microsoft.com/Start/2014/TaskbarLayout"
    Version="1">
   <CustomTaskbarLayoutCollection PinListPlacement="Replace">
    <defaultlayout:TaskbarLayout>
     <taskbar:TaskbarPinList>
      <taskbar:DesktopApp DesktopApplicationLinkPath="%APPDATA%\Microsoft\Windows\Start Menu\Programs\System Tools\File Explorer.lnk" />
      <taskbar:DesktopApp DesktopApplicationLinkPath="%ALLUSERSPROFILE%\Microsoft\Windows\Start Menu\Programs\Accessories\Wordpad.lnk" />
      <taskbar:DesktopApp DesktopApplicationLinkPath="%ALLUSERSPROFILE%\Microsoft\Windows\Start Menu\Programs\Firefox.lnk"/>
     </taskbar:TaskbarPinList>
    </defaultlayout:TaskbarLayout>
   </CustomTaskbarLayoutCollection>
</LayoutModificationTemplate>
```
Le fichier XML doit être mis à disposition sur un partage je l'ai donc mis dans mon SYSVOL de mon AD (C:\Windows\SYSVOL\sysvol\manon.rou\scripts)  
<img width="872" height="341" alt="image" src="https://github.com/user-attachments/assets/b5e8d8e5-1561-4d56-9a06-7cca9b943f9c" />  
Créer la GPO  
<img width="582" height="345" alt="image" src="https://github.com/user-attachments/assets/d35a9e00-5808-4f0d-a1d3-722e1eeb56d6" />  
Faire "Modifier" sur la GPO, puis dans Configuration utilisateur > Stratégies > Modèles d'administration > Menu Démarrer et barre des tâches double cliquer sur "Disposition de l'écran de démarrage".  
<img width="795" height="587" alt="image" src="https://github.com/user-attachments/assets/4202c1c2-55cb-490a-887e-4f80af469248" />  
Mettre les paramètres suivant (activer + chemin d'accès au fichier) puis faire OK.  
<img width="702" height="647" alt="image" src="https://github.com/user-attachments/assets/27ff6a46-f510-468b-a1cf-16d77839e958" />  
La barre des tâches est désormais configurée sur les 4 groupes sauf sur le serveur de gestion.  
  
- Désactivation Microsoft Store  
Créer une GPO  
<img width="761" height="542" alt="image" src="https://github.com/user-attachments/assets/5f0fb192-d471-4980-aa96-4184239c3fc0" />  
Faire "Modifier" sur la GPO, puis dans Configuration utilisateur, Stratégies, Modèles d’administration, Composants Windows, Windows Store double cliquer sur Désactiver l’application Windows Store.  
<img width="798" height="590" alt="image" src="https://github.com/user-attachments/assets/c5b49a1d-a671-4868-9fd7-adb51f45f8b8" />  
Double cliquer et faire activer puis ok  
<img width="692" height="647" alt="image" src="https://github.com/user-attachments/assets/ced2e83d-ef9c-4021-92e5-15aeb092fece" />  
Microsoft Store est désormais désactivé sur les 4 groupes sauf sur le serveur de gestion.  
  
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
