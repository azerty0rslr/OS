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
# Installer le rôle WDS
Install-WindowsFeature -Name WDS -IncludeManagementTools

# Installer les outils MDT (nécessite téléchargement manuel de MDT + ADK)
# Tu peux automatiser l’installation si les fichiers sont disponibles localement :
Start-Process "C:\Installers\MicrosoftDeploymentToolkit_x64.msi" -ArgumentList "/quiet" -Wait
Start-Process "C:\Installers\adksetup.exe" -ArgumentList "/quiet" -Wait
```


# Dossier de déploiement
New-Item -Path "D:\DeploymentShare" -ItemType Directory

# Partage MDT
New-SmbShare -Name "DeploymentShare$" -Path "D:\DeploymentShare" -FullAccess "Administrators"

# Importer l’image Windows dans MDT via PowerShell (il faut le module MDT)
Import-MDTOperatingSystem -Path "D:\DeploymentShare\Operating Systems" -SourcePath "D:\Sources\Windows11" -DestinationFolder "Win11"
