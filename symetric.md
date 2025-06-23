# Symetric

## Définitions de base

- Cryptologie : Science qui traite de la communication et du stockage sécurisé des données.
- Cryptographie : Pratique et étude des techniques de communication sécurisée en présence d’adversaires.
- Cryptanalyse : Analyse des systèmes d’information pour découvrir des aspects cachés, souvent pour obtenir le texte en clair sans la clé.
- Texte en clair (Plaintext, PT) : Information lisible directement, à protéger.
- Texte chiffré (Ciphertext, CT) : Information lisible uniquement avec la clé secrète.
- Chiffrement/Déchiffrement : Processus de transformation du texte en clair en texte chiffré et inversement, à l’aide d’un algorithme et d’une clé.
- Clé : Élément fondamental et secret de l’algorithme de chiffrement.

## Systèmes de chiffrement
- Chiffrement symétrique : Utilise la même clé pour chiffrer et déchiffrer. Toutes les parties doivent partager la clé, qui doit être transmise par un canal sécurisé avant la communication. Si la clé est interceptée, la sécurité est compromise.
- Chiffrement asymétrique (clé publique/privée) : Utilise deux clés différentes, une publique et une privée.
- Fonctionnement du chiffrement symétrique
- Les données utiles à chiffrer sont appelées « texte en clair », qu’il s’agisse de texte, images, musiques, etc.
- Le résultat du chiffrement est le « texte chiffré », souvent sous forme binaire.

## Étapes typiques du chiffrement :

- Choisir un algorithme de chiffrement (chiffre).
- Générer une clé de chiffrement.
- Générer un vecteur d’initialisation (IV).
- Choisir un mode d’opération.
- Choisir un type de bourrage (padding).
- Chiffrer le texte en clair avec l’algorithme, la clé et le padding.

## Avantages et inconvénients du chiffrement symétrique

- Avantages :
  - Très rapide, facile à implémenter matériellement.
  - Gestion des clés simplifiée (une seule clé).
- Inconvénients :
  - Tous partagent la même clé, donc impossible d’identifier l’origine d’un message (problème de non-répudiation).

## Types de chiffrements symétriques

- Chiffrements par blocs : Fonctionnent sur des blocs de données (exemple : AES avec des blocs de 128 bits). Si la longueur du texte n’est pas un multiple du bloc, on ajoute du padding. Le texte chiffré a toujours une taille multiple du bloc.
- Chiffrements par flot : Fonctionnent sur des octets ou bits individuels, sans besoin de padding ni de modes d’opération. Plus simples et rapides, mais souvent considérés comme moins sûrs que les chiffrements par blocs.

## Sécurité et taille des clés

- La sécurité d’un algorithme se mesure en « bits de sécurité » (ex : 256 bits signifie qu’il faudrait 2^256 opérations pour casser la clé par force brute).
- Plus la clé est longue, plus la sécurité est exponentiellement accrue.
- Recommandations du NIST (2021) :
  - 112 bits de sécurité suffisent jusqu’en 2030.
  - 128 bits de sécurité suffisent jusqu’à une avancée majeure (ex : informatique quantique).
- Les chiffrements symétriques modernes supportent généralement des clés jusqu’à 256 bits.

## Chiffres symétriques importants

- AES (Advanced Encryption Standard) : Standard actuel, rapide, sécurisé, bloc de 128 bits, clés de 128, 192 ou 256 bits. Résiste très bien aux attaques connues.
- DES & 3DES : Ancien standard, aujourd’hui considéré comme trop faible (clés de 56 bits pour DES, 112 ou 80 bits pour 3DES). À éviter dans les nouvelles applications.
- RC4 : Ancien chiffre par flot, rapide mais aujourd’hui considéré comme non sécurisé (ex : sécurité réduite à 26 bits pour une clé de 128 bits).
- ChaCha20 : Chiffre par flot moderne, sécurisé, rapide, clé de 256 bits, souvent préféré à AES sur les appareils sans accélération matérielle AES (ex : mobiles). Limité à 256 Go de données continues par clé/nonce.
- Autres chiffres supportés par OpenSSL : Blowfish, CAST5, GOST89, IDEA, RC2, RC5 (blocs de 64 bits, déconseillés pour de très longs textes), ARIA, Camellia, SEED, SM4 (blocs de 128 bits, adaptés aux longs textes).

## Chiffres nationaux

- Certains algorithmes sont développés et standardisés dans des pays spécifiques (ex : CAST5 au Canada), souvent utilisés pour des raisons réglementaires locales plus que pour leur popularité internationale.

## Conclusion

La cryptographie symétrique reste la méthode la plus rapide et la plus utilisée pour le chiffrement de données, surtout pour les gros volumes. AES est la référence actuelle, mais d’autres algorithmes existent pour des cas spécifiques. La sécurité dépend principalement de la longueur de la clé et de la solidité de l’algorithme choisi. L’arrivée de l’informatique quantique pourrait remettre en question certains standards, mais pour l’instant, des clés de 128 à 256 bits restent recommandées.
