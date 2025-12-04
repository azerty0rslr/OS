# Jour 1
## Installation 
### Changer nom PC
Dans les paramètres, changer le nom du PC puis le redémarrer.  
<img width="1056" height="793" alt="image" src="https://github.com/user-attachments/assets/11e8c040-e875-4b97-b03e-0cfe1feb295f" />  
<img width="813" height="650" alt="image" src="https://github.com/user-attachments/assets/bf0b9c46-5dd4-4b37-8e4f-70334820d496" />  

### Mettre une IP fixe
Dans la gestion des serveurs, aller dans Ethernet  
<img width="1030" height="767" alt="image" src="https://github.com/user-attachments/assets/cf96c58d-3081-4bae-a5fc-2a812a29e138" />  
<img width="1037" height="772" alt="image" src="https://github.com/user-attachments/assets/ee3f184a-8bc8-425b-8f20-06416c182708" />  
Puis aller dans propriétés  
<img width="492" height="502" alt="image" src="https://github.com/user-attachments/assets/18be95ec-3f0d-4874-976e-7a4861130cb1" />  
Faire propriétés de Protocole Internet version 4  
<img width="415" height="466" alt="image" src="https://github.com/user-attachments/assets/df3e18c5-65d9-4f54-bc92-f34e039a7274" />  

### Installation AD
Faire ajouter des rôles et fonctionnalités  
<img width="1032" height="773" alt="image" src="https://github.com/user-attachments/assets/61da4286-c62e-4606-a0cb-1fe4268b695d" />  
Faire suivant jusqu'à atteindre cette page  
<img width="792" height="558" alt="image" src="https://github.com/user-attachments/assets/aba2d97e-837f-4c22-ad51-96132e5e6e6a" />  
Puis ajouter les services sélectionnés et faire suivant.  
<img width="801" height="572" alt="image" src="https://github.com/user-attachments/assets/7042967d-dec5-4ad5-a210-2b217e7d2fd1" />  
Continuer jusqu'à l'installation.  
  
NE PAS OUBLIER DE CONFIGURER UN MOT DE PASSE POUR LA SESSION ADMINISTRATEUR.  
<img width="785" height="582" alt="image" src="https://github.com/user-attachments/assets/7fccb551-153a-4f33-8e92-24ec448459f0" />  
Puis continuer  
<img width="786" height="583" alt="image" src="https://github.com/user-attachments/assets/b982c548-b9ea-445a-9d58-b1924d5d4449" />  
Laissez les chemins par défaut :  
<img width="786" height="582" alt="image" src="https://github.com/user-attachments/assets/e3ce81c1-c3be-4b82-a2b3-7637330b6ac0" />  
Puis continuez jusqu'à l'installation.  
Le script généré est le suivant :  
<img width="770" height="415" alt="image" src="https://github.com/user-attachments/assets/2991fedc-c0fe-44c7-860a-c3ac45b8be07" />  
  
Doc très utile : https://www.it-connect.fr/creer-un-domaine-ad-avec-windows-server-2016/  

## WSUS
Windows Server Update Services : outils de gestion de mises à jour - centralisé, moins de bande passante, garanti la conformité et sécurité des systèmes.
Récupère les Maj depuis microsoft Update, les admins approuvent ou non ces mises à jour. 
WSUS est un rôle, même début que l'active directory.  
<img width="791" height="567" alt="image" src="https://github.com/user-attachments/assets/12fab298-1276-471b-af43-632c678aa54f" />  
On le stock dans un dossier précis pour qu'il soit localisé :  
<img width="792" height="567" alt="image" src="https://github.com/user-attachments/assets/2c0d20ad-a887-489e-b059-a65f8a7b501b" />  
On installe WSUS puis on redémarre. Pour vérifier qu'il a bien été installer vérifier dans Outils -> Service WSUS  
<img width="751" height="557" alt="image" src="https://github.com/user-attachments/assets/dd178129-20a5-420f-8bfc-d4c59713f82a" />  
Puis continuer jusqu'à Start Connecting (très long) :  
<img width="757" height="555" alt="image" src="https://github.com/user-attachments/assets/c2d5cdc5-cc74-4b8a-b084-a6e69b238969" />  
Une fois installé, retourner dans Outils -> Service WSUS, l'interface doit être le suivant :  
<img width="976" height="707" alt="image" src="https://github.com/user-attachments/assets/ab2f0463-9903-4dc0-be44-57f471ac8c32" />  


## Créer un Windows 11 Client 



