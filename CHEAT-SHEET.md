# Cheat Sheet

## Chiffrer/Déchiffrer un fichier avec AES

```
# Créer un fichier de test
seq 1000 > plaintext_file.txt

# Générer une clé
openssl rand -hex 32
# Générer un IV (vecteur d'initialisation)
openssl rand -hex 16

# Chiffrer le fichier
openssl enc -aes-256-cbc -K <clé_hexadécimale> -iv <iv_hexadécimal> -e -in plaintext_file.txt -out encrypted_file.enc

# Déchiffrer le fichier
openssl enc -aes-256-cbc -K <clé_hexadécimale> -iv <iv_hexadécimal> -d -in encrypted_file.enc -out decrypted_file.txt

# Vérif fichier identique
diff plaintext_file.txt decrypted_file.txt # ou vimdiff

cksum plaintext_file.txt
cksum decrypted_file.txt

openssl dgst -sha256 plaintext_file.txt
openssl dgst -sha256 decrypted_file.txt

# Mauvaise decryption
openssl enc -aes-256-cbc -K <clé_incorrecte> -iv <iv_hexadécimal> -d -in encrypted_file.enc -out fail_output.txt
```

## Générer mots de passe avec OpenSSL

```
openssl passwd PASSWORD
openssl passwd PASSWORD
openssl passwd PASSWORD
openssl passwd -salt uAhg9Cjk PASSWORD
openssl passwd -6 -salt uAhg9Cjk PASSWORD
```

## Calcul digest (empreinte)

```
# exemple de fichier openssl-3.0.16.tar.gz

# Méthode 1 :
echo "$(cat openssl-3.0.12.tar.gz.sha256) openssl-3.0.12.tar.gz" | sha256sum --check
sha256sum openssl-3.0.12.tar.gz

# Méthode 2 :
openssl dgst -sha256 openssl-3.0.12.tar.gz

# List des digests possibles dans OpenSSL :
openssl dgst -list

# Calculer une empreinte 100 fois (comparaison de performance)
time for i in {1..100}; do openssl dgst -sha256 openssl-3.0.12.tar.gz &>/dev/null; done
```

## Calcul HMAC

```
# Vérifier à la fois l'intégrité et l’authenticité d’un fichier grâce à une clé secrète

# Générer un fichier
seq 10000 > file.txt

# Générer une clé HMAC (256 bits)
openssl rand -hex 32

# Calcul HMAC avec openssl dgst
openssl dgst -sha256 -mac HMAC -macopt hexkey:0ae080ffa90b3dfd692746573a63a7af3876a1a55bef64e3534ee7327a0c695e file.txt

# Calcul HMAC avec openssl mac
openssl mac -digest SHA-256 -macopt hexkey:0ae080ffa90b3dfd692746573a63a7af3876a1a55bef64e3534ee7327a0c695e -in file.txt HMAC
```

## Derive key from password (KDF)

```
# Générer un salt
openssl rand -hex 16

# Dériver la clef avec SCRYPT
openssl kdf -keylen 32 -kdfopt 'pass:MySecretPa$$word' -kdfopt hexsalt:81aa4c39c451229a0c123a6af0099dfa -kdfopt n:65536 -kdfopt r:8 -kdfopt p:1 SCRYPT

# Variante, mot de passe au format HEX
# Convertir le mot de passe en HEX
echo -n 'MySecretPa$$word' | od -A n -t x1 | tr -d ' '

# Utiliser hexpass à la place de pass:
openssl kdf -keylen 32 -kdfopt hexpass:4d7953656372657450612424776f7264 -kdfopt hexsalt:81aa4c39c451229a0c123a6af0099dfa -kdfopt n:65536 -kdfopt r:8 -kdfopt p:1 SCRYPT
```

## RSA pairs in OpenSSL














