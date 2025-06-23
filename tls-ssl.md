# TLS/SSL

## Définitions et objectifs

- SSL (Secure Socket Layer) et TLS (Transport Layer Security) sont des protocoles cryptographiques assurant la sécurité des communications entre deux pairs (généralement client et serveur) au niveau des couches transport et application du modèle OSI.
- Ils garantissent la confidentialité des données, leur intégrité et l’authentification des pairs, principalement via TCP, bien que DTLS utilise UDP.

 ## Fonctionnement général

- SSL/TLS fonctionne en mode duplex, permettant l’échange sécurisé de données dans les deux sens.
- Le client initie la connexion, le serveur l’attend.

## Record Layer Protocol (RLP)

- Le RLP prend les données des couches supérieures, les découpe en blocs (SSLPlaintext), les compresse (optionnel), puis les chiffre.
- Les blocs sont ensuite authentifiés via un MAC (Message Authentication Code) basé sur MD5 ou SHA-1, puis envoyés via TCP.
- Les algorithmes de chiffrement utilisés incluent IDEA, RC4, RC2, DES, 3DES, et Fortezza.

## En-tête RLP

- L’en-tête contient le type de données, la version du protocole et la longueur du fragment.
- Le RLP ne gère pas la négociation des algorithmes ni l’échange de clés, qui sont pris en charge par le protocole de Handshake.

## SSL Handshake Protocol

- Permet l’authentification du serveur (et éventuellement du client), la négociation des suites cryptographiques, l’accord sur l’algorithme de compression, et l’établissement du "Master Secret" pour la session.
- L’authentification du serveur empêche les attaques de type "Man-In-The-Middle" en vérifiant l’identité du serveur via des certificats X.509 émis par une autorité de certification reconnue.
- L’authentification du client est optionnelle.

## Session vs Connexion

- Session : tunnel sécurisé entre deux pairs, pouvant contenir plusieurs connexions. Elle est identifiée par un Session ID, contient les certificats, la suite de chiffrement, l’algorithme de compression, le Master Secret, etc.
- Connexion : canal de communication unique (généralement une connexion TCP) associé à une session, avec ses propres paramètres (nombres aléatoires, clés de chiffrement, séquences, etc.).

## Échange de clés et établissement du secret

- Plusieurs méthodes d’échange de clés : RSA (avec ou sans authentification client), DSA, Diffie-Hellman (DH), etc.
- Le client et le serveur échangent des nombres aléatoires, le client génère un PreMasterSecret, le chiffre avec la clé publique du serveur, puis les deux calculent indépendamment le MasterSecret à l’aide de fonctions de hachage MD5 et SHA-1.
- Le MasterSecret sert ensuite à dériver les clés de chiffrement et d’authentification pour la session.

## Évolution de SSL/TLS

- SSL 1.0 : jamais déployé.
- SSL 2.0 : obsolète depuis 2011.
- SSL 3.0 : abandonné en 2015.
- TLS est le successeur direct de SSL, la première version (TLS 1.0) datant de 1999.
- Les différences majeures entre SSL et TLS incluent la gestion des erreurs, la prise en charge des algorithmes, les formats de messages et les fonctions cryptographiques utilisées.
- TLS 1.1 et 1.2 apportent des corrections et des améliorations (meilleure gestion des suites de chiffrement, prise en charge de SHA-2, extensions comme SNI, support des certificats OpenPGP, etc.).
- TLS 1.3 (RFC 8446) apporte des améliorations majeures en termes de performance et de sécurité :
  - Réduction du nombre d’échanges nécessaires pour établir une connexion sécurisée grâce à des mécanismes comme "TCP Fast Open" et "TLS False Start".
  - Introduction du 0-RTT (zéro round trip time) pour la reprise de session, permettant d’envoyer des données chiffrées dès le début, avec certaines limitations de sécurité.
  - Sécurité renforcée grâce à l’exigence de la Perfect Forward Secrecy (PFS) via Diffie-Hellman, empêchant le déchiffrement rétroactif en cas de compromission de la clé privée du serveur.

## Résumé des points clés

- SSL/TLS est essentiel pour la sécurité des communications sur Internet, en particulier pour le web.
- Il repose sur des mécanismes robustes d’authentification, de chiffrement et d’intégrité
- Le protocole a évolué pour répondre aux nouvelles menaces et exigences de performance, avec TLS 1.3 représentant l’état de l’art actuel.
