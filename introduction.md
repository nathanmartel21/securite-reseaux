# Introduction

## Présentation générale

OpenSSL est une boîte à outils open source de cryptographie qui comprend :
- Une bibliothèque SSL/TLS, destinée aux développeurs de logiciels.
- Des utilitaires en ligne de commande, utilisables par tous, qui reposent sur cette même bibliothèque.
SSL (Secure Sockets Layer) est un protocole créé pour sécuriser les communications sur des réseaux non sûrs. Il a été remplacé par TLS (Transport Layer Security) depuis 2015, mais le nom "OpenSSL" est resté par héritage historique, bien qu’il implémente désormais principalement TLS et ses dérivés (DTLS, etc.).

## Licence

- OpenSSL était historiquement sous licence de type BSD.
- Depuis la version 3.0, il est sous licence Apache 2.0, permettant son utilisation dans des applications open source comme propriétaires.

## Fonctionnalités et adoption

OpenSSL prend en charge de nombreux algorithmes cryptographiques :

- Chiffrement symétrique et asymétrique
- Signatures numériques
- Résumés de message (hash)
- Échange de clés
- Certificats X.509
- Protocoles SSL, TLS, DTLS, et autres technologies liées à la cryptographie

Il est largement adopté sur de nombreux systèmes d'exploitation, initialement sur Unix et ses variantes (Linux, BSD, macOS, etc.), mais aussi sur Windows, Android, iOS, MS-DOS, VMS, etc. Il a bénéficié d’optimisations matérielles pour les architectures x86, x86_64, ARM, ce qui en fait l’une des bibliothèques TLS/crypto les plus rapides et répandues. D’autres bibliothèques TLS proposent même des couches de compatibilité OpenSSL pour faciliter leur adoption via ses API.

## Historique

- Basé sur la bibliothèque SSLeay, développée dès 1995 par Eric Andrew Young et Tim Hudson.
- En 1998, après le départ des créateurs chez RSA, SSLeay est forké en OpenSSL par Mark Cox, Ralf Engelschall, Stephen Henson, Ben Laurie et Paul Sutton.
- La première version (0.9.1) sort en décembre 1998.
- Aujourd’hui, le développement est géré par le OpenSSL Management Committee, avec une vingtaine de contributeurs principaux, dont peu travaillent à temps plein.

## OpenSSL 3.0

- Version LTS (Long Term Support) jusqu’en 2026.
- Passage à la licence Apache 2.0.

- Introduction du concept de "providers" pour modulariser les implémentations d’algorithmes cryptographiques.
- Les anciens "engines" sont dépréciés au profit des providers.
- Support de providers tiers pour intégrer de nouveaux algorithmes.
- Introduction de Kernel TLS (KTLS) pour accélérer la transmission de données via le noyau.
- Ajout du support du Certificate Management Protocol, d’un client HTTP/HTTPS simple, de nouveaux algorithmes (AES-GCM-SIV, GMAC, KMAC, SSKDF, SSHKDF, etc.).
- Nouvelles APIs de haut niveau (EVP_MAC, EVP_KDF, EVP_RAND), dépréciation des APIs bas niveau.
- Suppression d’algorithmes jugés non sûrs par défaut, nettoyage du code, refonte de la gestion des erreurs, suppression du mode interactif de la CLI.

## Bibliothèques concurrentes

- GnuTLS : Pour les projets sous licence GPL, moins de fonctionnalités qu’OpenSSL, mais supporte OpenPGP. Moins performant, mais supporte tous les algorithmes populaires.
- NSS : Originaire du navigateur Netscape, utilisé par Firefox, Thunderbird, LibreOffice. Moins populaire qu’OpenSSL, mais bien documenté.
- Botan : Écrit en C++11/17, API C++ principale, bindings pour C, Python, Ruby, Rust, Haskell, Java, Ocaml. Moins populaire, mais bonne documentation.
- Librairies légères (IoT, embarqué) :
- wolfSSL : Très optimisée matériellement, plus rapide qu’OpenSSL selon le matériel, GPL 2.0.
- Mbed TLS : Sous licence Apache 2.0, très légère, peu d’algorithmes.
- MatrixSSL : Licence Apache 2.0, très légère.
- LibreSSL : Fork d’OpenSSL créé en 2014 par OpenBSD après la faille Heartbleed, code allégé, adoption limitée hors OpenBSD.
- BoringSSL : Fork d’OpenSSL par Google, code allégé, fonctionnalités spécifiques, pas de compatibilité API avec OpenSSL ni avec ses propres anciennes versions, réservé aux besoins de Google.

## État des bibliothèques SSL/TLS en 2025

- OpenSSL 3.x : Changements majeurs d’API, régressions de performance en multi-thread, pas de support QUIC natif, performances inférieures à OpenSSL 1.1.1.
- Alternatives :
  - AWS-LC : Très rapide, supporte QUIC, nécessite C11.
  - WolfSSL : Léger, certifiable FIPS, API compatible.
  - LibreSSL : Plus sécurisé mais moins complet et plus lent.
  - BoringSSL : Prêt pour QUIC, évolue rapidement, ruptures fréquentes de compatibilité.
- Benchmarks : AWS-LC est ~50% plus rapide qu’OpenSSL 1.1.1, WolfSSL a des performances comparables, OpenSSL 3.x est nettement moins rapide.
- QUIC : Supporté par AWS-LC, WolfSSL, LibreSSL ; OpenSSL n’a pas d’API compatible QUIC.
- Recommandations : Pour la performance et QUIC, adopter AWS-LC ; pour l’embarqué, WolfSSL ; en attendant, rester sur OpenSSL 1.1.1 avec des patchs QuicTLS.

## Conclusion

OpenSSL reste la référence en matière de bibliothèque cryptographique open source, mais ses alternatives se distinguent sur des critères de performance, de légèreté, de licence ou de fonctionnalités spécifiques (comme QUIC). Le choix dépend des besoins du projet (licence, performance, compatibilité, usage embarqué, etc.).
