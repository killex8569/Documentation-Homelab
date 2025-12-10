
# Introduction

## Les virtio ? c'est quoi ? 

## Pourquoi créer de cette manière ?



# Création de la VM


## Prérequis

Pour les prérequis pour créer la vm, il nous faudra au minimum : 

- Un node proxmox
- Une image iso de windows 10 ou 11
- La dernière version des virtio de Windows
- Un minimum de compétences sur proxmox et Windows.

## Création de la VM

Dans un premier temps, nous allons nous rendre sur l'interface pour créer une VM, vous aller renseigner les informations nécéssaires tel que l'id, le nom le node etc...

```{=latex}
\begin{center}
\includegraphics[width=0.8\linewidth]{data/general.png}

\vspace{0.2cm}
\textit{Figure X - Config général}
\end{center}
```

Ensuite dans l'onglet suivants, nous allons pouvoirs importer l'ISO et configurer la version de l'OS :

```{=latex}
\begin{center}
\includegraphics[width=0.8\linewidth]{data/vm.png}

\vspace{0.2cm}
\textit{Figure X - mise en place de l'OS}
\end{center}
```

Ici nous allons venir chercher dans un premier temps l'iso Windows qu'il nous faut (ici j'ai choisi une vm Windows 11 22H2), ensuite nous allons venir séléctionner le type d'os (Windows) ainsi que sa version. Après juste en dessous, nous allons activer les virtio, puis venir séléctionner le virtio le plus récent (ici le 0.1.285)

Après, dans l'onglet suivant, nous allons pouvoirs configurer le système :

```{=latex}
\begin{center}
\includegraphics[width=0.8\linewidth]{data/sys.png}

\vspace{0.2cm}
\textit{Figure X - Mise en place de l'OS}
\end{center}
```

Ici, nous allons pouvoirs activer l'agent QEMU (qui permet d'obtenir des informations lors du démarrage de la vm comme sont ip par exemple), Puis nous allons venir séléctionner l'endroit ou la partition EFI et la puce TPM vont pouvoirs s'installer et stocker des éléments (ici local-lvm).

Ensuite dans l'onglet `Disque`, nous allons venir configurer les paramètre suivant : 

- Device : Laissez SCSI par défaut !
- Storage : Local-lvm
- Taille du disque (64Go suffisent)


```{=latex}
\begin{center}
\includegraphics[width=0.8\linewidth]{data/disque.png}

\vspace{0.2cm}
\textit{Figure X - Configuration des disques}
\end{center}
```

Dans l'onglet CPU, vous aller devoirs modifier le nombre de coeurs mais aussi le type, comme ci-dessous : 

- Type : Hosts (par défaut, proxmox utilise cette version : x86-64-v2-AES)
- Coeurs : 2

```{=latex}
\begin{center}
\includegraphics[width=0.8\linewidth]{data/cpu.png}

\vspace{0.2cm}
\textit{Figure X - Configuration du CPU}
\end{center}
```

Pour la partie mémoire (4096 --> 4Go de ram) et réseau, vous pouvez laisser par défaut (juste pour le réseau, séléctionner le bon VLAN, le votre ou celui en commun pour tout le monde). Vous pouvez ensuite lancer votre machine.


## Démarrage de la VM

Une fois votre VM Lancer, vous allez devoirs boot sur windows (dans les 3 première secondes lors du boot, il faut cliquer sur n'importe qu'elle touche, sinon vous n'aller pas utiliser l'iso windows est vous aller devoirs reboot votre machine).

Une fois sur l'interface pour installer windows, cliquez sur installer puis "Je n'ai pas de clé de licence", séléctionner votre version de Windows. 

Une fois arriver sur l'installation de de windows, cliquez sur installation personnaliser (sinon cela ne fonctionnera pas).

Une fois sur cette onglet, normalement aucun disque n'est détécter et c'est normal ! Il faut aller dans "Charger un pilote" : 

```{=latex}
\begin{center}
\includegraphics[width=0.8\linewidth]{data/windows_install.png}

\vspace{0.2cm}
\textit{Figure X - Installation de Windows}
\end{center}
```

Une fois sur l'interface pour charger un driver, normalement plusieurs s'affiches, il faut choisir celui qui correspond à la version de votre windos (dans mon cas c'est un Windows 11, donc je dois prendre celui avec w11, si vous aviez voulu un Windows serveur 2022, vous auriez du séléctionner le 2K22)

```{=latex}
\begin{center}
\includegraphics[width=0.8\linewidth]{data/selection_version_win.png}

\vspace{0.2cm}
\textit{Figure X - Virtio Windows}
\end{center}
```

Puis cliquez sur suivant. 

Une fois Windows installer, normalement vous pouvez voir votre disque apparaitre, cliquer sur suivant une fois votre disque séléctionner l'installation se lance. Après le redémarrage, vous aller arriver sur l'interface graphique de Windows pour installer l'OS, configurer vos paramètres. Arriver à l'étape du réseau, normalement votre carte réseau ne vas pas être détécter, pour cela il suffit de cliquer sur installer le pilote et Windows le trouvera tt seul (si cela n'est pas le cas, vous pouvez changer la carte réseau pour E100 le temps de l'installation, une fois installer repasser sur virtio et installer les pilote décrit ci-dessous). Une fois que votre windows à été installer, vous allez pouvoirs normalement vous rendre dans le lecteur avec les virtio et éxécuter les deux paquets suivants : `virtio-win-gt-x64` et `virtio-win-guest-tools`, ce dernier permet l'utilisation de l'agent qemu ! 


# Bonne pratique



<!-- Commande pour créer le document : 

- pandoc Page\ de\ garde.md Sommaire.md main.md -o Rapport_Greenit_Alexandre_FA_FD_DAO.pdf --pdf-engine=xelatex -V lang=fr

-->
