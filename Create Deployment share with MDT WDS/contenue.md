# Introduction

Microsoft Déployment Tolls (MDT), et Windows Déployment Services sont deux services que nous pouvons installer sur un AD (Active Directory), nous permettant ainsi de déployer des OS Windows directement depuis le réseau. Cette puissante solution permet en plus de pouvoirs personnaliser notre infrastructure en intégrant des scripts et des applications. Dans notre production, nous avons effectivement placer des agents de supervision permettant de remonter et de superviser automatiquement les nouvelles machines créer sur le réseau.


# Installation de la solution de déploiment

Pour pouvoirs installer notre solutions, nous allons avoirs besoin du matériels suivants : 

- Un AD (Machine Windows serveur 1) en 172.16.10.85
- Une DéploymentShare qui sera connecter à l'AD (Machine Windows serveur 2) 172.16.10.100
- Un réseau dédié pour les déploiement (Dans notre cas ce sera le VLAN 1100)
- Une machine Virtuelle virger nous permettant de véirifer le fonctionnement de PXE et du déploiement via le réseau
- Un serveur proxmox permettant d'effectuer les testes adéquats


## Mise en place et configuration du serveur DHCP

Sur notre serveur DHCP présent sur notre AD, nous avons du faire certaines modification, notamment au niveau des options du DHCP. Dans un premier temps nous l'avons installer de manière classique avec les autres services nécéssaires (sur l'image ci-dessous ne figure pas l'installation du rôle ADDS car nous l'avons implémenter lors de l'installation préalable de Hello-my-dir et HardenAD).

Nous allons pouvoirs nous rendre dans "Géré --> Ajout de rôle et de fonctionnalités", sur cette page nous allons aller à l'endroits pour séléctionner les services à intaller (DHCP et DNS uniquement).


```{=latex}
\begin{center}
\includegraphics[width=0.9\linewidth]{data/dhcp_dns.png}

\vspace{0.2cm}
\textit{Figure X - Installation des services DNS et DHCP sur l'AD}
\end{center}
```

```{=latex}
\begin{center}
\includegraphics[width=0.9\linewidth]{data/dhcp_dns_2.png}

\vspace{0.2cm}
\textit{Figure X - Intégration et redémarrage de la machine}
\end{center}<>
```

Une fois installer, nous pouvons redémarrer la machine et enfin commencer la configuration du DHCP. Dans un premier temps nous allons nous rendre dans l'interface de configuration du DHCP ("Outils --> DHCP") et configurer ce dernier. En premier, nous allons créer une étandu, cette dernière nous permetteras de pouvoirs distribuer les adresses ip sur cette plage (Actuellement sur notre VLAN 1100 en 172.16.10.0/24). 

```{=latex}
\begin{center}
\includegraphics[width=0.9\linewidth]{data/dhcp.png}

\vspace{0.2cm}
\textit{Figure X - Configuration du service DHCP}
\end{center}
```

Par la suite faire une installation classique du service DHCP. Notre réseau de notre VLAN est en 172.16.10.0/24 et notre passerelle en 172.16.10.2 qui est notre Firewall Watchguard. 

Une fois notre service DHCP actifs, nous allons passer à la configuration de notre service DNS. Dans un premier temps, nous allons pouvoirs faire en sorte d'activer le service DNS. Pour cela dans "Outils --> DNS" nous allons créer une **nouvelle zone principal**. Nous allons renseigner les éléments suivants : 

- Zone principale
- Nom de la zone : networks.local

Puis dans la zone de recherche inversé : 
- Zone principal
- Zone de recherche inversé ipv4
- ID réseau : 172.17.10

Une fois ces deux éléments mis en place, nous allons pouvoirs configurer les options du DHCP, pour cela nous allons nous rendre dans "Outils --> DHCP --> notre étandu --> Option"
 
```{=latex}
\begin{center}
\includegraphics[width=0.9\linewidth]{data/dhcp_option.png}

\vspace{0.2cm}
\textit{Figure X - Configurations des options du DHCP}
\end{center}
```

Une fois le DNS configurer, nous allons pouvoirs passer à l'étape suivante qui est d'implémenter les services de déploiements sur la seconde machine.



## Ajout de MDT et WDS sur la seconde machine

Sur notre deuxième machine, nous allons pouvoirs mettre en place MDT et WDS qui sont les deux services qui nous permettent de pouvoirs déployer via le réseau une image disque bootable. L'objectifs est de déployer une image .wim à travers le réseau permettant sont déploiement.





# Mise en place d'application déployer avec l'OS


# Création d'une image ISO permettant le déploiement en mode hors ligne de la solution


