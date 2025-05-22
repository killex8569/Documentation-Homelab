# Introduction

Microsoft Déployment Tolls (MDT), et Windows Déployment Services sont deux services que nous pouvons installer sur un AD (Active Directory), nous permettant ainsi de déployer des OS Windows directement depuis le réseau. Cette puissante solution permet en plus de pouvoirs personnaliser notre infrastructure en intégrant des scripts et des applications. Dans notre production, nous avons effectivement placer des agents de supervision permettant de remonter et de superviser automatiquement les nouvelles machines créer sur le réseau.


# Installation de la solution de déploiment

Pour pouvoirs installer notre solutions, nous allons avoirs besoin du matériels suivants : 

- Un AD (Machine Windows serveur 1)
- Une DéploymentShare qui sera connecter à l'AD (Machine Windows serveur 2)
- Un réseau dédié pour les déploiement
- Une machine Virtuelle virger nous permettant de véirifer le fonctionnement de PXE et du déploiement via le réseau


## Mise en place du serveur DHCP

Sur notre serveur DHCP présent sur notre AD, nous avons du faire certaines modification, notamment au niveau des options du DHCP. Dans un premier temps nous l'avons installer de manière classique avec les autres services nécéssaires (sur l'image ci-dessous ne figure pas l'installation du rôle ADDS car nous l'avons implémenter lors de l'installation préalable de Hello-my-dir et HardenAD).

Nous allons pouvoirs nous rendre dans "Géré --> Ajout de rôle et de fonctionnalités", sur cette page nous allons aller à l'endroits pour séléctionner les services à intaller (DHCP et DNS uniquement).


```{=latex}
\begin{center}
\includegraphics[width=0.9\linewidth]{data/}

\vspace{0.2cm}
\textit{Figure X - Installation des services sur l'AD}
\end{center}
```

# Mise en place d'application déployer avec l'OS


# Création d'une image ISO permettant le déploiement en mode hors ligne de la solution


