# Introduction

Dans cette réalisation, nous allons apprendre à mettre en place un serveur DNS et DHCP nous permettant ainsi d'automatiser l'installation et l'utilisation des machines virtuelle dans proxmox. Nous choisirons ici d'utiliser le service DHCP et DNS d'Opnsense par rapport à ces même services qu'un ctive Directory (Windows serveur) car 



## Ce que l'on vas apprendre dans cette réalisation

Dans ce tuto, nous allons apprendre à réaliser une installation et une configuration propre de ces services, à la fin de ce tuto vous serez capable de mettre en place : 

- Mise en place d'un serveur DHCP sur Opnsense
- Mise en place d'un serveur DNS sur Opnsense
- Utilisation du service DNS via le VPN
- Compréhension du fonctionnement d'un DHCP et DNS

# Mise en place du serveur DNS

# Configuration du Parefeu pour permettre l'utilisation du DNS via VPN 

# Mise en place d'un serveur DHCP

# Ajout de règle de firewalling

Pour pouvoirs utiliser les services DNS, nous avons besoin de pouvoirs contacter le serveur sur le port 53 (Ceci n'est pas obligatoirement utiles aujourd'hui car la majorité des logiciels de nos jours utilisent du DoH (DNS over HTTPS) qui permet d'utiliser les services via le port 443 donc via internet.) Cependant pour sécuriser le fonctionnement de ce dernier, nous allons devoirs créer une règle de Parefeu dans les deux réseau en question permettant d'ouvrir le port 53 mutuellement.

```{=latex}
\begin{center}
\includegraphics[width=0.9\linewidth]{data/users_2.png}

\vspace{0.2cm}
\textit{Figure X - Mise en place d'une règle de filtrage sur le VLAN tuto autorisant le trafic DNS}
\end{center}
```


# Conclusion



```{=latex}
\begin{center}
\includegraphics[width=0.9\linewidth]{data/users_2.png}

\vspace{0.2cm}
\textit{Figure X -}
\end{center}
```