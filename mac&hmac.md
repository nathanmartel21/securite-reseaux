# Résumé : MAC & HMAC

## Introduction

Un MAC (Message Authentication Code ou code d’authentification de message) est une courte séquence de bits (par exemple, 256 bits) qui authentifie un message. Il permet au destinataire de vérifier que le message provient bien de l’expéditeur déclaré et qu’il n’a pas été modifié pendant le transfert. Pour générer un MAC, l’expéditeur utilise un message et une clé secrète ; le destinataire doit disposer du même message et de la même clé pour vérifier le MAC.

## Différences avec d’autres mécanismes

### MAC vs Message Digest

- Un message digest (empreinte de message) garantit l’intégrité, mais n’est pas protégé contre la falsification.
- Un MAC offre à la fois l’intégrité et l’authenticité, car la clé secrète empêche un attaquant de recalculer un MAC valide pour un message modifié.

### MAC vs Signature numérique

- Les signatures numériques utilisent la cryptographie asymétrique et offrent la non-répudiation (l’auteur ne peut nier avoir signé).
- Les MAC utilisent la cryptographie symétrique (même clé pour l’expéditeur et le destinataire) et ne fournissent pas la non-répudiation.

## Sécurité des MAC

Un MAC est considéré comme sécurisé s’il résiste aux attaques de falsification :

- Falsification universelle : créer un MAC valide pour n’importe quel message.
- Falsification sélective : créer un MAC valide pour un message spécifique choisi à l’avance.
- Falsification existentielle : trouver un couple message/MAC valide, même aléatoirement.
- Attaque par oracle : obtenir des MACs pour des messages choisis et tenter de forger un nouveau couple message/MAC.

La sécurité d’un MAC dépend de la sécurité de la fonction sous-jacente et de la clé secrète utilisée.

## HMAC : MAC basé sur une fonction de hachage

- HMAC (Hash-based Message Authentication Code) est un MAC généré à partir d’une fonction de hachage cryptographique et d’une clé secrète.

### Formule :

```
HMAC(K, message) = H((K' ⊕ opad) || H((K' ⊕ ipad) || message))
```

- H : fonction de hachage (ex : SHA-256)
- K : clé secrète
- K' : clé adaptée à la taille du bloc de la fonction de hachage
- ipad et opad : valeurs de remplissage spécifiques

### Sécurité de HMAC :

- Niveau de sécurité = min(longueur de la clé, longueur du hash)
- Si la clé a une faible entropie (ex : mot de passe simple), la sécurité est réduite.

## Combinaison MAC et chiffrement

Trois schémas principaux :

- Encrypt-then-MAC (EtM) : on chiffre, puis on calcule le MAC sur le texte chiffré (préféré pour la sécurité).
- Encrypt-and-MAC (E&M) : on chiffre, puis on calcule le MAC sur le texte en clair.
- MAC-then-Encrypt (MtE) : on calcule le MAC sur le texte en clair, puis on chiffre le tout.

Le principe de « Cryptographic Doom » recommande de toujours vérifier le MAC avant toute opération cryptographique.

### Modes d’authentification modernes

Les modes d’encryptage authentifié (ex : AES-GCM, ChaCha20-Poly1305) intègrent l’authentification et n’ont plus besoin d’un MAC séparé.

### Utilisation dans les protocoles

- TLS : historiquement MtE, puis passage à EtM, et enfin, avec TLS 1.3, seuls les chiffrements authentifiés sont utilisés. HMAC reste utilisé pour certaines fonctions internes.
- SSH : utilisait E&M, puis passage à EtM avec de nouveaux algorithmes MAC, et supporte désormais aussi les chiffrements authentifiés.
- IPsec : utilise EtM depuis le début et a adopté les chiffrements authentifiés par la suite.
