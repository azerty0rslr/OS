## Le code et son explication : 

	# trouve puis écrit le nom d'utilisateur
    $NomUtilisateur = [System.Security.Principal.WindowsIdentity]::GetCurrent().Name
    Write-Output "Bienvenue $NomUtilisateur"
    
    # écrit le nom de la machine en utilisant la variable $env:COMPUTERNAME
    Write-Output "Nom de la machine : $env:COMPUTERNAME"
    
    # Stocke dans une variable $DateHeure la date et l'heure puis les affiches
	$DateHeure = Get-Date
	Write-Output "Date et heure actuelles : $DateHeure"
	
	# Récupère la dernière date de démarrage (par Get-CimInstance)
	$DernierDemarrage = (Get-CimInstance -ClassName Win32_OperatingSystem).LastBootUpTime 
	$Demarrage = (Get-Date) - $DernierDemarrage 
	# Affiche le temps écoulé depuis le dernier démarrage 
	Write-Output "Temps écoulé depuis le dernier démarrage : $($Demarrage.Days) jours, $($Demarrage.Hours) heures, $($Demarrage.Minutes) minutes"
	
	# Récupère les données du répertoire C, le converti en GB (car .Free contient les données en octets) puis affiche l'espace libre en GB
	$C = Get-PSDrive -Name C
	$EspaceLibre = [math]::Round($C.Free / 1GB, 2)
	Write-Output "Espace libre sur C: $EspaceLibre GB"
	
	#Ajout d'une pause pour que le fichier ne se ferme pas immédiatement
	Read-Host "Appuyez sur Entrée pour fermer la fenêtre"

## Captures d'écran du fonctionnement :

![[Pasted image 20241021142516.png]]
## Comment configurer le script pour qu'il démarre en même temps que la session :

Dans le ***planificateur de tâches***, *créer une nouvelle tâche*.
Dans l'onglet ***Général*** : la nommer, (sélectionner *Exécuter avec les autorisations maximales* pour être sûre de l'exécution du script dès le démarrage )
Dans l'onglet ***Déclencheur*** puis *Nouveau* : choisir *Au démarrage* et laisser les options définies par défaut 
Dans l'onglet ***Actions*** puis *Nouveau* : dans *Programme/Script* écrire *powershell.exe* et dans *Ajouter des arguments (facultatif)* entrer le chemin d'accès au fichier du script (enregistré en .ps1) sous la forme suivante -> -ExecutionPolicy Bypass -NoExit -File "C:\Users\Admin\Desktop\Start.ps1"
Dans l'onglet ***Conditions*** : supprimer les conditions liées à l'alimentation
Puis valider avec ***Ok***
