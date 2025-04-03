# Pour les VMs : 
- réseau hôte privé
- booter VMware (le disque)
- copier la session 

# Création AD1 :

Changer le nom du PC
	Rename-Computer -NewName "ActiveDirectory1" -Force -Restart
Réseau -> Ethernet -> changer l'IP (192.168.56.21 et .104) -> pour le DNS de l'AD2 mettre l'IP de l'AD1
	New-NetIPAddress -InterfaceIndex 6 -IPAddress 192.168.56.21 -PrefixLength 24
	Set-DnsClientServerAddress -InterfaceAlias "Ethernet" -ServerAddresses "192.168.56.21", "8.8.8.8"

## Déploiement AD DS :
Installer AD-Domain-Services
	Install-WindowsFeature -Name AD-Domain-Services -IncludeManagementTools
Importer le module
	 Import-Module ADDSDeployment
Installer les outils de gestion
	Add-WindowsFeature RSAT-ADDS
Vérifier que module bien chargé
	 Get-Module -ListAvailable ADDSDeployment
Lancer les commandes de déploiement d'AD DS :
	Install-ADDSForest `
	  -CreateDnsDelegation:$false `
	  -DatabasePath "C:\Windows\NTDS" `
	  -DomainMode "WinThreshold" `
	  -DomainName "manon.rou" `
	  -DomainNetbiosName "MANON" `
	  -ForestMode "WinThreshold" `
	  -InstallDns:$true `
	  -LogPath "C:\Windows\NTDS" `
	  -NoRebootOnCompletion:$false `
	  -SysvolPath "C:\Windows\SYSVOL" `
	  -Force:$true
Faire les vérifications post installation :
	Get-Service adws,kdc,netlogon,dns
	Get-ADDomainController
	Get-ADDomain manon.rou



# Création AD2 : 

Renommer le PC
	Rename-Computer -NewName "ActiveDirectory2" -Force -Restart
Affecter IP et DNS
	New-NetIPAddress -InterfaceIndex 6 -IPAddress 192.168.56.104 -PrefixLength 24
	Set-DnsClientServerAddress -InterfaceAlias "Ethernet" -ServerAddresses "192.168.56.21"

## Déploiement AD DS :
	Install-WindowsFeature -Name AD-Domain-Services -IncludeManagementTools
	Import-Module ADDSDeployment
	Get-Module -ListAvailable ADDSDeployment
Pas oublier de faire sysprep :
	cd C:\Windows\System32
	cd sysprep
	.\sysprep.exe
Cocher généraliser
OK

Suite AD DS : 
	Install-ADDSDomainController `
	  -NoGlobalCatalog:$false `
	  -CreateDnsDelegation:$false `
	  -Credential (Get-Credential) `
	  -CriticalReplicationOnly:$false `
	  -DatabasePath "C:\Windows\NTDS" `
	  -DomainName "manon.rou" `
	  -InstallDns:$true `
	  -LogPath "C:\Windows\NTDS" `
	  -NoRebootOnCompletion:$false `
	  -SiteName "Default-First-Site-Name" `
	  -SysvolPath "C:\Windows\SYSVOL" `
	  -Force:$true

![[Pasted image 20250128120222.png]]

# Création des OU (Unités organisationnelles) - sur AD1 :

New-ADOrganizationalUnit -Name "Utilisateurs" -Path "DC=manon,DC=rou" -ProtectedFromAccidentalDeletion $false
New-ADOrganizationalUnit -Name "Groupes" -Path "DC=manon,DC=rou" -ProtectedFromAccidentalDeletion $false
New-ADOrganizationalUnit -Name "Ordinateurs" -Path "DC=manon,DC=rou" -ProtectedFromAccidentalDeletion $false

# Création de groupes - sur AD1 :

New-ADGroup -Name "Admins" -GroupScope Global -Path "OU=Groupes,DC=manon,DC=rou"
New-ADGroup -Name "Utilisateurs" -GroupScope Global -Path "OU=Groupes,DC=manon,DC=rou"
# Création des utilisateurs - sur AD1 : 

New-ADUser -Name "Manon ROUSSELIERE" `
  -GivenName "Manon" `
  -Surname "ROUSSELIERE" `
  -SamAccountName "mrou" `
  -UserPrincipalName "mrou@manon.rou" `
  -Path "OU=Utilisateurs,DC=manon,DC=rou" `
  -AccountPassword (ConvertTo-SecureString "Motdep@sse123" -AsPlainText -Force) `
  -Enabled $true

#### Modification de son mot de passe et ajout au groupe admins (+ vérifications)

Set-ADAccountPassword -Identity "mrou" -NewPassword (ConvertTo-SecureString "RTYfghvbn@666" -AsPlainText -Force) -Reset
Add-ADGroupMember -Identity "Admins" -Members "mrou"
Get-ADGroupMember -Identity "Admins"

![[Pasted image 20250209085408.png]]

# Gestion de partage de fichiers - sur AD1:

New-Item -Path "C:\Partage" -ItemType Directory
New-SmbShare -Name "Partage" -Path "C:\Partage" -FullAccess "Admins"

![[Pasted image 20250209085806.png]]

# GPO - sur AD1 :

![[Pasted image 20250209102324.png]]


![[Pasted image 20250209102612.png]]

![[Pasted image 20250209102831.png]]
# AD2 rejoins le domaine (en temps que contrôleur):

Install-ADDSDomainController `
  -NoGlobalCatalog:$false `
  -CreateDnsDelegation:$false `
  -Credential (Get-Credential) `
  -CriticalReplicationOnly:$false `
  -DatabasePath "C:\Windows\NTDS" `
  -DomainName "manon.rou" `
  -InstallDns:$true `
  -LogPath "C:\Windows\NTDS" `
  -NoRebootOnCompletion:$false `
  -SiteName "Default-First-Site-Name" `
  -SysvolPath "C:\Windows\SYSVOL" `
  -Force:$true

#### Vérification
Get-Service adws,kdc,netlogon,dns
Get-ADDomainController
Get-ADDomain manon.rou

![[Pasted image 20250209100728.png]]

# Client1 va sur le domaine : 
Rename-Computer -NewName "Client1" -Force -Restart
Add-Computer -DomainName "manon.rou" -Credential MANON\Admin -Restart

#### Vérification
Test-Connection -ComputerName AD1

![[Pasted image 20250209101446.png]]


