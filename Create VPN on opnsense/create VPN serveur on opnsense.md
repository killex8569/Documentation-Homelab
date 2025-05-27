# Introduction
Dans un objectifs d'accéder au réseau depuis un accès distant, notre objectifs est de mettre en place un serveur OpenVPN qui nous permet en tant qu'utilisateurs (mobile et pc) de pouvoris accéder au réseau interne. 

Le serveur VPN nous permet ici de pouvoirs se connecter directement au Firewall et donc d'avoir la possibilité de géré les routes, les moyens de communication etc... directement via des règles de parefeu classique. Cette option est bien plus pratique et utilisée que de créer un serveur OpenVPN sur une machine.

A la fin de ce tuto, vous serez en mesure de mettre les éléments suivants en place sur votre infrastructure : 

- Mise en place d'un accès VPN pour votre réseau
- Mise en place de règle de gestion en fonction de votre 
- Compréhension du fonctionnement des certificats
- Mise en place de route permettant au VPN d'accéder au autres VLAN

# Création du serveur OpenVPN sur Opnsense

## Introduction
Pour commencer, vous allez avoir besoin de mettre à jour votre Parefeu Opnsense. Pour cela vous pouvez vous rendre dans **"Systeme --> Firmware --> Statut"** puis cliquez sur **"rechercher les mises à jours"**. Une fois que votre parefeu est à jour et à redémarrer, nous allons pouvoirs commencer l'installation et la mise en place du serveur OpenVPN (j'utilise ici uniquement un serveur OpenVPN car je considère que c'est la solution la plus répendu mais aussi la plus facile à mettre en place sur plusieurs appareils différent.)


## Création des certificats et autorité

Pour pouvoirs utiliser votre VPN, vous allez devoirs créer des utilisateurs. Chaque utilisateurs devront possèder un mot de passe et un certificat lui permettant de se déclarer comme un client pour le serveur VPN. Cependant en plus de tous cela, il nous est nécéssaire de devoirs créer une authorité de certification serveur qui sera utiliser par Opnsense pour valider la connexion. Rendez-vous dans **Système --> Trust --> Autorité"**. Une fois rendu dans cette onglet, vous allez pouvoirs créer votre premier certificat.



```{=latex}
\begin{center}
\includegraphics[width=0.9\linewidth]{data/CA_opnsense.png}

\vspace{0.2cm}
\textit{Figure X - Ajout d'un certificat coté serveur'}
\end{center}
```

Une fois sur cette page, nous allons pouvoirs créer notre autorité de certification, cliquez sur **"+"**. Puis renseigner les informations suivantes : 

- **Nom** : CA_Opnsense (par exemple)
- **Méthode** : "Create an internal Certificate Authority"
- **Type de clé** : Vous pouvez le laisser par défaut (par défaut : RSA-2048)
- **Algorithme de Hachage** : Vous pouvez le laisser par défaut (par défaut : SHA256)
- **Durée de vie** : 825

```{=latex}
\begin{center}
\includegraphics[width=0.9\linewidth]{data/CA_opnsense_1.png}

\vspace{0.2cm}
\textit{Figure X - Ajout d'un certificat coté serveur'}
\end{center}
```

Une fois le certificat ajouter, nous allons pouvoirs ajouter un certificat serveur. Pour cela rendez vous dans la section **"Système --> Trust --> Certificat"**. Puis cliquez sur **"+"** et renseigner les informations suivantes : 

```{=latex}
\begin{center}
\includegraphics[width=0.9\linewidth]{data/CA_opnsense_2.png}

\vspace{0.2cm}
\textit{Figure X - Ajout d'un certificat coté serveur'}
\end{center}
```

**Attention** : Pensez bien à renseigner l'émetteur comme votre autorité de certificat créer juste avant, ne laisser pas par défaut "auto-signer". Normalement les informations du certificat devrait se remplir automatiquement une fois votre emetteur choisi ! (comme par exemple le pays ou la régions.)


## Création du serveur OpenVPN

Très bien une fois que tout les pré-requis on été réunni, nous allons pouvoirs commencer. En premier, nous allons nous rendre dans la section **"VPN --> OpenVPN --> serveur"**. En passant par le wizard, il y aura la possibilité de créer automatiquement les 2 règles nécessaires pour autoriser le flux provenant des clients VPN.Sinon, il faudra les ajouter à la main, ça fonctionne très bien aussi.

- Type de serveur d'authentification : Local User Access
- Authorité de certification : Votre_autorité_de_certification
- Certificat : votre_certificat
- Dans la partie General OpenVPN Server Information, renseigner les paramètres suivants

```{=latex}
\begin{center}
\includegraphics[width=0.9\linewidth]{data/CA_opnsense_3.png}

\vspace{0.2cm}
\textit{Figure X - Ajout d'un certificat coté serveur'}
\end{center}
```

Une fois cela fait, nous allons pouvoirs créer les utilisateurs qui nous permetterons de pouvoirs nous connecter au VPN. Pour cela rendez vous dans **"System --> Access --> Users"**, une fois cela fait **créer votre user et revenez sur la page affichant la liste de utilisateurs et cliquez sur le bouton "certificat"**.  


```{=latex}
\begin{center}
\includegraphics[width=0.9\linewidth]{data/users.png}

\vspace{0.2cm}
\textit{Figure X - Ajout d'un utilisateur pour la connexion VPN}
\end{center}
```



# Conclusion

Vous savez maintenant comment créer un serveur OpenVPN sur votre firewall Opnsense vous permettant ainsi d'accéder à votre SI depuis l'extérieur.



Pour créer cette documentation, je me suis aider de ce tuto en ligne retrouvable [ICI](https://www.aukfood.fr/opnsense-configuration-dun-vpn-ssl-road-warrior-avec-pki/)