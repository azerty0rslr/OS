# Histoire apple
2008 : premier MacBook
2004 : iBook/PowerBook G4
1999 : iBook
1989 : Macintosh
## OS
1980-1998 : Finder
1991-1999 : macOS
2001-2015 : MacOS X
2016-2024 : macOS
# Interface utilisateur 
Menu, Dock (app, app récentes, handoff, dossier/contrôle), Barre des taches et centre de contrôle
Légères différences sur le clavier, raccourcis claviers différents
**App natives à Apple** : Mission control (pour plusieurs bureaux virtuels), modes focus, finder (= Fichiers), time machine (sauvegardes/copies du PC/d'un volume du PC), utilitaire de disque (efface, partitionne le(s) disque(s))
Dernières arrivées :widgets et stage manager
**VM sur Mac** : Parallels Desktop, VMware Fusion, VirtualBox
**Bureau distants** : protocole VNC -> Partage d'écran (app native), remotix
# Partition système
L’Utilitaire de disque sur Mac prend en charge plusieurs formats de système de fichiers :
- *MacOS Journalisé HFS* : depuis MacOS 8
- *Apple File System (APFS)* : Le système de fichiers utilisé par macOS 10.13 ou ultérieur.
- *Mac OS étendu* : Le système de fichiers utilisé par macOS 10.12 ou antérieur.
- *MS-DOS (FAT) et ExFAT* : Les systèmes de fichiers qui sont compatibles avec Windows.
# Développement OS
Langages : Objective-C, Haskell, Ruby, Python, C#, Caml, Swift
2014 : Software Development Kit (compilation d’outils permettant le développement d’applications destinées à un matériel/logiciel donné ou codées dans un langage de programmation spécifique.)
Logiciels : Xcode (permet de simuler n'importe quel appareil apple), TestFlight (test la stabilité de l'application, la mettre sur l'AppStore, lancer des Beta, )

# Compatibilité 
Lors de mise à jour d'OS, certains anciens produits Apple ne peuvent avoir accès à la dernière MAJ.
=> OpenCore Patcher : permet de patcher les anciens appareils sur les nouvelles versions (télécharge image disque de l'app, installe le patch et un volume d'amorçage pour la compatibilité)
