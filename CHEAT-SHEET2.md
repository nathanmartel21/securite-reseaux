## 1

```
# générer autorité de certif
openssl req -newkey ED448 -x509 -subj "/CN=Root CA" -addext "basicConstraints=critical,CA:TRUE" -days 3650 -noenc -keyout ca_keypair.pem -out ca_cert.pem
# générer clefs + CSR pour le serv TLS
openssl req -newkey ED448 -subj "/CN=localhost" -addext "basicConstraints=critical,CA:FALSE" -noenc -keyout server_keypair.pem -out server_csr.pem
# signer le certif du serv avec le CA
openssl x509 -req -in server_csr.pem -copy_extensions copyall -CA ca_cert.pem -CAkey ca_keypair.pem -days 3650 -out server_cert.pem
# lancer le serv TLS
openssl s_server -port 4433 -key server_keypair.pem -cert server_cert.pem

# iptables
# bloquer tout par défaut
sudo iptables -P INPUT DROP
# autoriser les connexions établies
sudo iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT
# autoriser localhost
sudo iptables -A INPUT -p tcp -s 192.168.88.X --dport 4433 -j ACCEPT
# autoriser pc prof
sudo iptables -A INPUT -p tcp -s 192.168.88.XXXXXXXX --dport 4433 -j ACCEPT
# iptables -nL -v

#### OU :

iptables -A INPUT -p tcp --dport 4433 -s 172.20.10.4 -j ACCEPT
iptables -A INPUT -p tcp --dport 4433 -s 172.20.10.5 -j ACCEPT
iptables -A INPUT -p tcp --dport 4433 -j DROP

# se connecter au serv
# lancer un serveur python et faire un curl dessus pour récupérer ca_cert.pem
openssl s_client -connect 192.168.88.XXXXXXXXXXXXXXXX:4433 -verify_return_error -verify_hostname localhost -CAfile ca_cert.pem
```

## 2 & 7

```
# Lancer deuxième machine
## Sur PC client :
vim /etc/network/interfaces

auto enp0s3
iface enp0s3 inet static
  address 192.168.2.2
  netmask 255.255.255.0
  gateway 192.168.2.1
  dns-nameservers 8.8.8.8 1.1.1.1

systemctl restart networking
reboot

## Sur routeur :

vim /etc/network/interfaces
# laisser dhcp pour la pont

auto enp0s8
iface enp0s8 inet static
  address 192.168.2.1
  netmask 255.255.255.0

systemctl restart networking
reboot

echo 1 > /proc/sys/net/ipv4/ip_forward
echo "net.ipv4.ip_forward = 1" >> /etc/sysctl.conf
sysctl -p
sudo apt update
sudo apt install iptables-persistent
iptables -t nat -A POSTROUTING -o enp0s3 -j MASQUERADE
iptables-save > /etc/iptables/rules.v4
```

## 3

```
# Sur le client
mkdir web
cd web
# Génération fichier
seq 1000 > plaintext_file.txt
# Clef AES :
openssl rand -hex 32
# IV :
openssl rand -hex 16
# Chiffrer le fichier
openssl enc -aes-256-cbc -K KEY -iv IV -e -in plaintext_file.txt -out encrypted_file.enc
# Générer la somme :
openssl dgst -sha256 encrypted_file.enc > encrypted_file.enc.sha256
python3 -m http.server

# Sur le routeur :
sudo iptables -t nat -A PREROUTING -i [INT EXTERNE] -p tcp --dport 8888 -j DNAT --to-destination 192.168.2.2:8000
sudo iptables -A FORWARD -p tcp -d 192.168.2.2 --dport 8000 -j ACCEPT

# Tester sur internet : http://[IP ROUTEUR]:8888

# Depuis PC prof :
curl http://[IP ROUTEUR]:8888/encrypted_file.enc
curl http://[IP ROUTEUR]:8888/encrypted_file.enc.sha256
# Vérifier le hash
openssl dgst -sha256 encrypted_file.enc #comparer avec le fichier encrypted_file.enc.sha256

#Bonus, déchiffrement :
openssl enc -aes-256-cbc -K KEY -iv IV -d -in encrypted_file.enc -out decrypted.txt
```

## 4

```
# Lancer kali linux et mettre dans la conf une IP statique et la gateway 192.168.2.1 (IP routeur)
ip -c a && ping -c 1 192.168.2.1 && ping -c 1 192.168.2.2
# Rediriger le trafic entre les deux victimes :
echo 1 | sudo tee /proc/sys/net/ipv4/ip_forward
# Terminal 1 : (spoof vers la victime)
sudo arpspoof -i eth0 -t 192.168.2.2 192.168.2.1
# Terminal 2 : (spoof vers le routeur, il croit que kali est la victime)
sudo arpspoof -i eth0 -t 192.168.2.1 192.168.2.2
# Laisser les deux commandes en parralèle et lancer wireshark
# Lancer aussi la commande suivante et attendre d'avoir les deux lignes (les deux IPs pointent vers la m^me @MAC)
arp -a
```

## 5

```
# Sur le routeur
apt install mitmproxy
mkdir -p ~/.mitmproxy
cp ../1er/ca_cert.pem mitmproxy-ca-cert.pem
cp ../1er/ca_keypair.pem mitmproxy-ca-key.pem
# par défaut mitmproxy va contacter le serveur d'origine sur le port 443, d'où la redirection
sudo iptables -t nat -A PREROUTING -i [INT INTERNE] -p tcp --dport 443 -j REDIRECT --to-port 8080
echo 1 | sudo tee /proc/sys/net/ipv4/ip_forward
sudo mitmproxy --mode transparent --showhost --listen-port 8080

# Sur le client : faire https://[IP ROUTEUR]:8080
# Il y aura une erreur 502 mais on voit dans le mitmproxy
```













