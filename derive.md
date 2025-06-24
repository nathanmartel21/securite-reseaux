# Derive

## Différence entre mot de passe et clé de chiffrement

Le document explique qu’une clé de chiffrement symétrique est une séquence secrète de bits utilisée directement par un algorithme de chiffrement, par exemple une clé de 256 bits. Ces clés sont difficiles à manipuler pour les humains car elles ressemblent à des données aléatoires et sont impossibles à mémoriser. À l’inverse, les mots de passe ou phrases de passe sont plus conviviaux et faciles à retenir, ce qui explique pourquoi de nombreux logiciels de chiffrement permettent d’utiliser des mots de passe à la place des clés brutes. Toutefois, un mot de passe ne peut pas être utilisé directement par un algorithme de chiffrement : il faut d’abord en dériver une clé à l’aide d’une fonction de dérivation de clé (KDF).

## Fonction de dérivation de clé (KDF)

- Une KDF est une fonction qui produit une clé secrète de la longueur souhaitée à partir d’un autre secret, comme un mot de passe, une phrase de passe ou une combinaison de clés asymétriques. Le secret d’origine est appelé Input Key Material (IKM) et la clé produite Output Key Material (OKM). Les KDF utilisent généralement des fonctions de hachage cryptographique ou des opérations de chiffrement en bloc.
- PBKDF : Fonction de dérivation de clé basée sur un mot de passe
- Les PBKDF (Password-Based Key Derivation Function) sont des KDF conçues pour générer des clés à partir de secrets à faible entropie, comme les mots de passe. Elles servent aussi à hacher les mots de passe de manière plus résistante aux attaques par force brute que les simples fonctions de hachage.

## Paramètres d’une KDF

- IKM : le secret de départ, par exemple un mot de passe.
- Salt : donnée aléatoire ajoutée pour empêcher les attaques par tables arc-en-ciel ; recommandée d’au moins 128 bits et générée indépendamment du mot de passe.
- Info : information spécifique à l’application, utile pour lier la clé à un usage précis.
- Fonction PRF sous-jacente : HMAC ou fonction de chiffrement en bloc.
- Paramètres de résistance à la force brute : souvent un nombre d’itérations, mais certains KDF, comme scrypt, ajoutent des paramètres pour la mémoire et le CPU.
- Longueur de la clé souhaitée.

## Exigences d’une bonne KDF

- Déterministe : le même IKM et les mêmes paramètres donnent toujours la même clé.
- Irréversible : il doit être impossible de retrouver le IKM à partir de la clé dérivée.
- Résistante à la force brute.

## Entropie et sécurité

Les mots de passe ont beaucoup moins d’entropie que les clés générées aléatoirement. Par exemple, un mot de passe de 8 caractères (avec 80 possibilités par caractère) donne environ 51 bits d’entropie, tandis qu’un mot de passe de 12 caractères atteint 76 bits. Aucune PBKDF ne peut augmenter l’entropie d’un mot de passe, mais elle peut rendre les attaques par force brute plus coûteuses en multipliant les itérations ou en augmentant l’utilisation mémoire.

## Utilisation dans OpenSSL

OpenSSL 3.0 propose plusieurs KDF, mais seules scrypt et PBKDF2 sont adaptées à la dérivation de clés à partir de mots de passe :

- PBKDF2 (décrit par le standard PKCS#5) utilise HMAC (par exemple HMAC-SHA-256) et permet de régler le nombre d’itérations, ce qui la rend coûteuse en calcul mais pas en mémoire. OWASP recommandait en 2021 d’utiliser 310 000 itérations avec HMAC-SHA-256.
- Scrypt est le choix recommandé car il est à la fois coûteux en calcul et en mémoire, ce qui le rend plus résistant aux attaques utilisant du matériel spécialisé. Scrypt permet d’ajuster le volume de calcul, la mémoire et le parallélisme. En 2021, OWASP recommandait N=65 536, r=8, p=1.

Scrypt est aussi utilisé pour le hachage des mots de passe sur les systèmes GNU/Linux modernes et comme preuve de travail dans certaines cryptomonnaies (Litecoin, Dogecoin). Les KDF à étape unique comme HKDF, ANSI X9.42, ANSI X9.63 ou les PRF TLS/SSH ne sont pas adaptées comme PBKDF car elles ne sont pas assez coûteuses en calcul et ne prennent pas de paramètres de résistance à la force brute.

## Conclusion

Pour dériver une clé de chiffrement à partir d’un mot de passe, il est indispensable d’utiliser une PBKDF robuste comme scrypt ou PBKDF2, en configurant des paramètres adaptés pour résister aux attaques modernes. Scrypt est actuellement la meilleure option grâce à sa résistance accrue liée à l’utilisation intensive de la mémoire et du calcul.
