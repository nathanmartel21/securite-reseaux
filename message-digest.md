# Message digest

## Définition et propriétés des fonctions de hachage cryptographique

Une fonction de hachage cryptographique est un algorithme qui transforme un message de taille arbitraire en une empreinte (digest) de taille fixe, généralement courte (par exemple, 256 bits). Cette empreinte sert de somme de contrôle cryptographiquement forte. Une bonne fonction de hachage cryptographique doit être :

- **Déterministe** : le même message produit toujours la même empreinte.
- **Irréversible** : il doit être impossible de retrouver le message original à partir de l’empreinte.
- **Résistante aux collisions** : il doit être infaisable de trouver deux messages différents ayant la même empreinte.
- **Effet d’avalanche** : toute modification, même minime, du message doit entraîner un changement radical de l’empreinte.

## Applications principales

- **Vérification de l’intégrité des données** : permet de vérifier que des données n’ont pas été altérées lors d’un transfert ou d’un stockage.
- **HMAC (Hash-based Message Authentication Code)** : authentification des données en combinant une clé secrète et une fonction de hachage.
- **Signature numérique** : ce n’est pas le message entier qui est signé, mais son empreinte.
- **Protocoles sécurisés** : des protocoles comme TLS et SSH utilisent des HMACs et des signatures numériques pour sécuriser les échanges.
- **Vérification de mot de passe** : les systèmes modernes stockent des empreintes de mots de passe salées (ajout d’une donnée aléatoire appelée « sel ») plutôt que les mots de passe eux-mêmes.
- **Identification de contenu** : les empreintes servent à identifier de manière unique des objets, comme des fichiers ou des commits dans Git, ou des certificats X.509.
- **Blockchain et cryptomonnaies** : les blocs sont chaînés par des empreintes, assurant l’intégrité de la chaîne et empêchant la modification des blocs anciens.
- **Proof-of-work** : système où un client doit résoudre un défi cryptographique, souvent utilisé dans le minage de cryptomonnaies ou pour limiter les attaques par déni de service.

## Attaques et sécurité

Les principales attaques contre les fonctions de hachage sont :

- **Attaque par collision** : trouver deux messages différents avec la même empreinte.
- **Attaque de préimage** : retrouver un message à partir d’une empreinte donnée.

Le niveau de sécurité est mesuré en « bits de sécurité » (par exemple, 128 bits signifie qu’une attaque par collision nécessite \(2^{128}\) essais).

## Fonctions de hachage courantes et leur sécurité

- **SHA-2 (dont SHA-256, SHA-512, etc.)** : actuellement les plus utilisées, résistantes aux collisions connues, utilisées dans TLS, SSH, Bitcoin, Git, etc. SHA-256 produit une empreinte de 256 bits, résistance de 128 bits.
- **SHA-3** : sélectionnée via un concours NIST pour diversifier les algorithmes disponibles. Basée sur Keccak, elle est plus lente que SHA-2 mais offre une architecture différente. Peu utilisée actuellement, sauf dans Ethereum.
- **SHA-1** : obsolète, vulnérable aux collisions (première collision publique en 2017), plus utilisée dans les nouveaux systèmes.
- **Famille MD (MD2, MD4, MD5, MD6)** : conçue par Ronald Rivest, toutes aujourd’hui considérées comme non sécurisées. Les attaques sur MD5 et MD4 permettent de trouver des collisions en quelques secondes sur un PC standard.

## Évolution et migration

- Migration progressive des anciens algorithmes (SHA-1, MD5) vers des fonctions plus sûres (SHA-2, SHA-3).
- SHA-3 n’est pas destiné à remplacer immédiatement SHA-2, mais à offrir une alternative en cas de découverte de failles dans SHA-2.

## Résumé technique

Les fonctions de hachage cryptographique sont essentielles pour la sécurité informatique moderne, assurant l’intégrité, l’authenticité et l’identification unique des données. Leur sécurité repose sur la difficulté de trouver des collisions ou de retrouver le message original. L’évolution des attaques pousse à migrer vers des algorithmes plus récents et plus sûrs, comme SHA-2 et SHA-3, tandis que les anciens comme SHA-1 et MD5 sont progressivement abandonnés.
