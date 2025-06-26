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

# se connecter au serv
openssl s_client -connect 192.168.88.XXXXXXXXXXXXXXXX:4433 -verify_return_error -verify_hostname localhost -CAfile ca_cert.pem
```

## 2




















