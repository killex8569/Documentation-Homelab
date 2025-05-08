# Introduction
Dans cette documentation, nous allons explorer les fonctionnalités offertes par le module SDN intégré à Proxmox VE. Nous étudierons en détail son fonctionnement, ainsi que les paramétrages nécessaires sur l’infrastructure réseau, notamment sur un switch manageable, si vous en possédez un.

Nous aborderons également l’intégration des VLANs dans OPNsense, permettant la mise en place de règles de filtrage spécifiques à chaque VLAN. Cela vous permettra de segmenter efficacement votre réseau et de contrôler les flux inter-VLAN selon des politiques de sécurité adaptées.

# Partie 1 : Proxmox

## Introduction
Depuis la version 7.0 de Proxmox VE, un module nommé SDN (Software Defined Networking) a été intégré. Il permet la création et la gestion centralisée de réseaux virtuels, de VLANs, de routage inter-nœuds, ainsi que l’utilisation de technologies avancées comme les overlays (VXLAN, EVPN). Cela offre une abstraction réseau complète à l’échelle du cluster, bien plus souple et évolutive que les bridges traditionnels (vmbr), configurés localement sur chaque nœud.

Par défaut, Proxmox utilise un bridge réseau nommé vmbr0 (soit la sortie vers WAN), associé à une interface physique, pour permettre la connectivité des machines virtuelles.Il agit ainsi comme le réseau principal initial. 

La configuration de VLAN (Virtual LAN) est une pratique fondamentale dans toute architecture réseau, que ce soit pour un environnement domestique avancé (homelab) ou une infrastructure d’entreprise. Les VLAN permettent une segmentation logique du réseau, c’est-à-dire la séparation de domaines de broadcast. Cette segmentation peut ensuite être complétée par des règles de pare-feu ou des ACL (Access Control Lists) pour restreindre les communications entre les segments.

Les VLAN sont également un prérequis pour la mise en place de zones démilitarisées (DMZ). Une DMZ est un sous-réseau isolé conçu pour héberger des systèmes exposés à Internet, comme des serveurs web, wikis, portails SaaS ou autres services publics, tout en minimisant les risques pour le réseau interne. Elle n'est cependant efficace que si elle est combinée à une politique de filtrage rigoureuse et à une architecture sécurisée (pare-feu, IDS/IPS, reverse proxy, etc.).

Dans ce document, nous allons nous concentrer exclusivement sur la création et la configuration de VLANs dans Proxmox à l’aide du module SDN. L’implémentation de DMZ, ainsi que les considérations de sécurité réseau, ne seront pas couvertes ici.


## Ajout de VLAN sur proxmox


Pour commencer à créer un VLAN, rendez-vous sur **l’interface web de Proxmox VE**, puis accédez à :`Datacenter → SDN → Zones`. C’est à cet endroit que vous allez pouvoir définir une **zone SDN**, qui servira de base pour l’organisation de vos réseaux virtuels. Dans cet exemple, nous allons créer une zone nommée **vlandoc**. Cliquez sur **Add**, puis sélectionnez le type de zone **VLAN**. Cette étape permet de définir le **contexte de gestion réseau** pour les VLANs que vous utiliserez ensuite. Chaque zone peut regrouper plusieurs VLANs et configurations réseau associées.


```{=latex}
\begin{center}
\includegraphics[width=0.7\linewidth]{data/sdn_vlan.png}

\vspace{0.2cm}
\textit{Figure X - Création d'une zone de type VLAN}
\end{center}
```

```{=latex}
\begin{center}
\includegraphics[width=0.7\linewidth]{data/create_vlan.png}

\vspace{0.2cm}
\textit{Figure X - Information  de création d'une zone VLAN}
\end{center}
```

Une fois la zone SDN créée, vous devriez voir apparaître votre nouvelle zone dans l’interface. Vous pouvez désormais vous rendre dans l’onglet **VNets** de Proxmox. C’est ici que vous allez pouvoir créer vos VLANs. Commencez par cliquer sur **Create**, puis renseignez un nom pour votre VNet. Dans cet exemple, nous allons le nommer **VLAN67**. Ce VNet sera rattaché à la zone **vlandoc** que nous avons précédemment créée. Attribuez ensuite un **tag VLAN** — dans notre cas, le tag **67**, ce qui signifie que nous créons ici le VLAN 67. Vous pouvez bien entendu choisir un autre identifiant si nécessaire. Il est **fortement recommandé** d’inclure le numéro du VLAN dans le nom du VNet (ex. : `VLAN67`) afin d’éviter toute confusion lors de la gestion de plusieurs VLANs. Avant de valider la création, ouvrez les paramètres avancés (**Advanced**) et activez l’option **VLAN Aware**. Cette option permet au VNet de prendre en charge le transit de VLANs. Bien que cela ne soit **pas obligatoire**, il peut être utile de l’activer si vous prévoyez d'utiliser ce VNet pour regrouper plusieurs VLANs sur un bridge unique. Cela dépend de votre architecture réseau et de votre usage.

Vous pouvez maintenant valider la création du VNet.


```{=latex}
\begin{center}
\includegraphics[width=0.7\linewidth]{data/vlan.png}

\vspace{0.2cm}
\textit{Figure X - Création d'une première VLAN}
\end{center}
```

Une fois le VNet créé, vous disposez désormais de votre premier VLAN configuré — dans notre exemple, le **VLAN 67**. Pour que les modifications soient prises en compte par Proxmox, retournez dans l’onglet **SDN**, puis cliquez sur **Apply**. Cette étape est nécessaire pour appliquer les changements de configuration réseau effectués dans le module SDN.


```{=latex}
\begin{center}
\includegraphics[width=0.7\linewidth]{data/apply.png}

\vspace{0.2cm}
\textit{Figure X - Appliquer les changements pour intégrer les VLAN}
\end{center}
```


```{=latex}
\begin{center}
\includegraphics[width=0.7\linewidth]{data/available.png}

\vspace{0.2cm}
\textit{Figure X - Disponibilité des VLAN}
\end{center}
```

Une fois les modifications appliquées, votre **zone SDN** ainsi que vos **VLANs** sont créés et prêts à être utilisés. Lors de la première création d’une zone ou d’un VNet, un **court délai** peut être nécessaire avant que les éléments soient disponibles sur tous les nœuds. Cela dépend notamment du temps de propagation de la configuration dans le cluster Proxmox. Par la suite, lors de la création d’une **machine virtuelle (VM)** ou d’un **container (CT)**, vous aurez la possibilité de sélectionner l’interface réseau à utiliser. Vous pourrez choisir entre le bridge réseau par défaut (`vmbr0`) ou l’un des **VNets SDN** que vous venez de créer.

---

> **Remarque** :  
> Dans cet exemple, j’utilise un **cluster Proxmox composé de deux nœuds**. C’est pourquoi certains éléments peuvent apparaître en double dans les captures d’écran (un par nœud).


# Partie 2 : Configuration sur le switch

## Déclaration des VLAN sur le switch



# Partie 3 : Intégration sur Opnsense

## Intégration des VLAN

## Ajout des interfaces

## Mise en fonctionnement des interfaces

## Mise en place du DHCP

## Ajout de règle de parefeu adéquats



# Test et améliorations



# Conclusion

### info

Document généré et créer en markdown retrouvable [ici]()

Commande de création : `pandoc page\ de\ garde.md sommaire.md introduction.md contenue.md conclusion.md -o Creer_un_VLAN_avec_le_SDN_sur_proxmox.pdf --pdf-engine=xelatex -V lang=fr`