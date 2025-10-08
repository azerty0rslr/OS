## Sources
### GPO 
https://komjordan.fr/posts/adgpo.html
https://www.it-connect.fr/chapitres/comment-creer-sa-premiere-gpo/
https://learn.microsoft.com/en-us/answers/questions/2039079/group-policy-on-domain-controller-vm-to-run-a-run
https://learn.microsoft.com/fr-fr/windows/iot/iot-enterprise/get-started/quickstart-sysprep-capture-deploy?tabs=physicaldevice
https://learn.microsoft.com/fr-fr/windows-server/identity/ad-ds/manage/group-policy/group-policy-management-console


### Sysprep 
https://learn.microsoft.com/fr-fr/windows-hardware/manufacture/desktop/sysprep-command-line-options?view=windows-11


### Active Directory
https://www.linuxtricks.fr/wiki/windows-server-installer-active-directory-adds-en-powershell
https://learn.microsoft.com/fr-fr/windows-server/identity/ad-ds/manage/active-directory-domain-join-permissions
https://myitknowledge.fr/creer-gerer-ou-active-directory/
https://www.it-connect.fr/troubleshooting-gpo-utilisation-de-gpresult/
https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/deploy/install-active-directory-domain-services--level-100-

### MDT 
https://www.it-connect.fr/cours-tutoriels/administration-systemes/windows-server/deploiement-mdt-wds/


### Déployer l'image sur  au moins 2 postes via MDT au WDS
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

### Joindre les postes au domaine Active Directory

```
# Joindre le nom de domaine
netdom join %Cam% /domain:camelia.local /userd:Administrateur /passwordd:Admin1234
```
```
# Redemarrer le poste
shutdown /r /t 0
```

```
# Créer des OU spécifiques (Stagiaires, IT, Direction).
dsadd ou "OU=Stagiaires,DC=camelia,DC=local"
```
GPO étape manuel
créé et lier la GPO
Apporter les modifications
Configuration ordinateur -> Paramètres Windows -> Paramètres de sécurité

Pour un mot de passe plus fort aller dans  Stratégies de compte -> Stratégie de mot de passe
Verouiller session après inactivité Configuration utilisateur -> Modèles d'administration -> Composants Windows → Session -> Délai d’inactivité

### GPO Restriction accés panneau de configuration et paramete windows

``` Powershell
Set-GPRegistryValue -Name "Sécurité de base" `
 -Key "HKCU\Software\Microsoft\Windows\CurrentVersion\Policies\Explorer" `
 -ValueName "NoControlPanel" -Type DWord -Value 1
```

### mise en place d'applocker 
```
Set-GPRegistryValue -Name "AppLocker_Politique" `
 -Key "HKLM\Software\Policies\Microsoft\Windows\SrpV2" `
 -ValueName "EnforcementMode" -Type DWord -Value 1
```
```
# Stagiaure
New-AppLockerPolicy -RuleType Executable -User "Stagiaires" -ReferenceFilePath "C:\Program Files" -RuleName "Blocage_Stagiaires" -Action Deny

#IT
New-AppLockerPolicy -RuleType Executable -User "IT" -ReferenceFilePath "*" -RuleName "Autorisation_IT" -Action Allow
```

### Bloquer les cles USB
```
# appel de la GPO
Get-GPO -All

# Bloquer le péripherique usb pour tout les postes
Set-GPRegistryValue -Name "Sécurité de base" `
 -Key "HKLM\SYSTEM\CurrentControlSet\Services\USBSTOR" `
 -ValueName "Start" -Type DWord -Value 4
```
Créé une autre GPO Pour les IT et ajouter cela :
```
# débloquer pour le IT
Set-GPRegistryValue -Name "USB_IT_Autorisé" `
 -Key "HKLM\SYSTEM\CurrentControlSet\Services\USBSTOR" `
 -ValueName "Start" -Type DWord -Value 3
```
Dépoiment de préférence GPO (lecteur réseaux, imprimante par défaut, ...)

Vérifier et personnaliser les règles du pare-feu Windows Defender (inbound/outbound)
```
netsh advfirewall firewall show rule name=all
```
Vérifier et personnaliser les règles du pare-feu Windows Defender (in/out).

```
# Vérifier les régles existantes
netsh advfirewall firewall show rule name=all

#création d'une régle example autorisation d'utilisation d'un port
netsh advfirewall firewall add rule name="Autoriser RDP" dir=in action=allow protocol=TCP localport=3389

# création d'une régle bloquer un port
netsh advfirewall firewall add rule name="Bloquer FTP" dir=out action=block protocol=TCP remoteport=21
```

Sénario de dépanage BCD, SFC
```
bootrec /fixmbr
bootrec /fixboot
bootrec /scanos
bootrec /rebuildbcd
sfc /scannow
 ```
Commande suppression un dossier corompu dans un profil
```
regedit
```
Vérifier BranchCache 

```
netsh branchcache show status all
```
Vérification de l'Event Viwer :

Journaux Windows > Application / Systémé > GroupPolicy > eventvwr.msc

Pour les teste complementaire on peut :

  - Commande pour tester la latence 'Ping'
  - Commande 'Tracert' pour voir le chemin réseau

Mettre en place un serveur WSUS, approuver des mises à jour et forcer leur application via GPO.

```
#Installer WSUS
Install-WindowsFeature -Name UpdateServices, UpdateServices-WidDB, UpdateServices-Services -IncludeManagementTools

# crée le dossier de stockage
New-Item -Path "C:\WSUS" -ItemType Directory

# lancer la configuration WSUS
"C:\Program Files\Update Services\Tools\wsusutil.exe" postinstall CONTENT_DIR=C:\WSUS

# appouver les mise a jour
$wsus = Get-WsusServer
$updates = $wsus.GetUpdates() | Where-Object { $_.UpdateClassificationTitle -eq "Critical Updates" -and $_.IsApproved -eq $false }
$group = $wsus.GetComputerTargetGroups() | Where-Object { $_.Name -eq "Tous les ordinateurs" }

foreach ($update in $updates) {
    $update.Approve($group, [Microsoft.UpdateServices.Administration.ApprovalAction]::Install)
}

# creartion du lien entre la GPO et OU
New-GPLink -Name "GPO_WSUS" -Target "OU=Serveurs,DC=camelia,DC=local"
```
Déploiement d’un script de connexion/déconnexion. 
```
# connexion
$User = $env:USERNAME
$Time = Get-Date -Format "yyyy-MM-dd HH:mm:ss"
$Computer = $env:COMPUTERNAME
$LogPath = "C:\Logs\SessionLog.txt"

Add-Content -Path $LogPath -Value "$Time - Connexion de $User sur $Computer"

#Déconnexion
$User = $env:USERNAME
$Time = Get-Date -Format "yyyy-MM-dd HH:mm:ss"
$Computer = $env:COMPUTERNAME
$LogPath = "C:\Logs\SessionLog.txt"

Add-Content -Path $LogPath -Value "$Time - Déconnexion de $User sur $Computer"
```
le script doit se stocker dans -> Configuration utilisateur > Paramètres Windows > Scripts (ouverture/fermeture de session)

on utilisera : 
```
powershell.exe -ExecutionPolicy Bypass -File "\\Serveur\Scripts\Session\connexion.ps1"

# ou

powershell.exe -ExecutionPolicy Bypass -File "\\Serveur\Scripts\Session\deconnexion.ps1"
```
