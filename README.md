# tp-6-yanis692
tp-6-yanis692 created by GitHub Classroom

# Exercice 1. Adressage IP (rappels)
Vous administrez le réseau interne 172.16.0.0/23 d’une entreprise, et devez gérer un parc de 254 machines
réparties en 7 sous-réseaux. La répartition des machines est la suivante :
- Sous-réseau 1 : 38 machines
- Sous-réseau 2 : 33 machines
- Sous-réseau 3 : 52 machines
- Sous-réseau 4 : 35 machines
- Sous-réseau 5 : 34 machines
- Sous-réseau 6 : 37 machines
- Sous-réseau 7 : 25 machines
Donnez, pour chaque sous-réseau, l’adresse de sous-réseau, l’adresse de broadcast (multidiffusion) ainsi
que les adresses de la première et dernière machine configurées (précisez si vous utilisez du VLSM ou pas).

172.16.0.0/26 -> sous réseaux 3 (52 machines), adresse de broadcast : 172.16.0.63/26, première adresse : 172.16.0.1/26, dernière adresse : 172.16.0.62/26 
172.16.0.64/26 -> sous réseaux 1 (38 machines), adresse de broadcast : 172.16.0.127/26, première adresse : 172.16.0.65/26, dernière adresse : 172.16.0.126/26
172.16.0.128/26 -> sous réseaux 6 (37 machines), adresse de broadcast : 172.16.0.191/26, première adresse : 172.16.0.129/26, dernière adresse : 172.16.0.190/26 //
172.16.0.192/26 -> sous réseaux 4 (35 machines), adresse de broadcast : 172.16.0.255/26, première adresse : 172.16.0.193/26, dernière adresse : 172.16.0.254/26 //
172.16.1.0/26 -> sous réseaux 5 (34 machines), adresse de broadcast : 172.16.1.63/26, première adresse : 172.16.1.1/26, dernière adresse : 172.16.1.62/26 //
172.16.1.64/26 -> sous réseaux 2 (33 machines), adresse de broadcast : 172.16.1.127/26, première adresse : 172.16.1.65/26, dernière adresse : 172.16.1.126/26
172.16.1.128/27 -> sous réseaux 7 (25 machines), adresse de broadcast : 172.16.1.159/27, première adresse : 172.16.1.129/27, dernière adresse : 172.16.1.158/27

# Exercice 2. Préparation de l’environnement

2. lo est une boucle locale.

3. Dans les versions récentes, Ubuntu installe d’office le paquet cloud-init lors de la configuration du système. Ce paquet permet la configuration et le déploiement de machines dans le cloud via un script au démarrage. Nous ne nous en servirons pas et sa présence interfère avec certains services (en particulier le changement de nom d’hôte) ; par ailleurs, vos machines démarreront plus rapidement. Désinstallez complètement ce paquet (il faudra penser à le faire également sur le client ensuite.)
```
sudo apt-get purge cloud-init
```

4. Les deux machines serveur et client se trouveront sur le domaine tpadmin.local. A l’aide de la commande hostnamectl renommez le serveur (le changement doit persister après redémarrage, donc cherchez les bonnes options dans le manuel !). On peut afficher le nom et le domaine d’une machine avec les commandes hostname et/ou dnsdomainname ou en affichant le contenu du fichier /etc/hostname. 
```
sudo hostnamectl set-hostname tpadmin.local --static
```
![image](https://user-images.githubusercontent.com/77662970/192343127-af3adf29-00f0-47b2-8d4b-22fd88f6384a.png)

# Exercice 3. Installation du serveur DHCP

1. Sur le serveur, installez le paquet isc-dhcp-server. La commande systemctl status isc-dhcp-server devrait vous indiquer que le serveur n’a pas réussi à démarrer, ce qui est normal puisqu’il n’est pas encore configuré (en particulier, il n’a pas encore d’adresses IP à distribuer).
```
sudo apt-get install isc-dhcp-server
```

2. Un serveur DHCP a besoin d’une IP statique. Attribuez de manière permanente l’adresse IP 192.168.100.1 à l’interface réseau du réseau interne. Vérifiez que la configuration est correcte.
```
sudo nano /etc/netplan/50-cloud-init.yaml
```
```
network:
  version: 2
  renderer: networkd
  ethernets:
    ens192:
      dhcp4: true
    ens224
      addresses:
        - 192.168.100.1/24
```
```
sudo netplan try
```
```
sudo netplan apply
```
```
sudo ip link set ens224 up
```
```
ip a
```

3. Les 2 premieres lignes permettent d'inclure du temps avant l'expiration de l'adresse ip.

![image](https://user-images.githubusercontent.com/77662970/193455559-a560a1fe-3b31-4c4d-b43d-b088b7db4195.png)


4. 
```
INTERFACESv4="ens224"
```
5. 

![image](https://user-images.githubusercontent.com/77662970/192467902-2c2f936a-c997-4159-8f2f-5b0d06516f6c.png)

6.

![image](https://user-images.githubusercontent.com/77662970/193458849-b94e2335-6bfe-4a19-abde-8c6f73cdad49.png)


![image](https://user-images.githubusercontent.com/77662970/192484959-c97f12ad-ab8a-44a8-85b6-9912ed4e49f3.png)

7. 

![image](https://user-images.githubusercontent.com/77662970/193459834-169b0b2c-a4a4-40db-ac06-85106e12d8af.png)

DHCPDISCOVER : sert à détecter les serveurs DHCP disponibles
DHCPOFFER : répond à un paquet DHCPDISCOVER
DHCPREQUEST : correspond à une requête du client
DHCPACK : ne réponse du serveur. Elle inclut des paramètres et l'adresse IP du client


8. Le fichier dhcp-lease-list contient différentes informations sur les requête et le clients.
La commande
``` 
sudo dhcp-lease-list
```
affiche différentes informations sur le clients :

![image](https://user-images.githubusercontent.com/77662970/193460087-b7e4c922-3824-455b-8632-5f2f96cd86c8.png)

9.
```
ping 192.168.100.100
```
![image](https://user-images.githubusercontent.com/77662970/193460290-9eec4a5b-1787-4b5d-b3d6-cc098d73998e.png)

10.
On rajoute cette configuration aux fichier dhcpd.conf
![image](https://user-images.githubusercontent.com/77662970/193463773-12ae750b-03ca-4352-9b6a-22704aa7d418.png)

puis on applique les changement avec ```dhcpd -t``` puis on restart le serveur, on regarde le status de celui-ci si tout est bon on passe coter client pour faire ```dhclient -v``` puis ```ip a``` et normalement la configuration est réussie :

![image](https://user-images.githubusercontent.com/77662970/193464040-36da97a6-c47f-4124-bc27-7bdecc996b34.png)

# Exercice 4

1.

![image](https://user-images.githubusercontent.com/77662970/193464456-bc7ab673-c57d-4007-b5b1-09463dd9e80d.png)

2.
On rentre cette commande coter serveur 
```
sudo iptables --table nat --append POSTROUTING --out-interface ens192 -j MASQUERADE
```
puis on ping
![image](https://user-images.githubusercontent.com/77662970/193465356-6d3be6c9-c3f9-4072-9882-1ad059de7d8b.png)

# Exercice 5

1.

![image](https://user-images.githubusercontent.com/77662970/193465587-6e03e7f4-b12f-4434-8438-1d2a3cb515fe.png)

2.

![image](https://user-images.githubusercontent.com/77662970/193465714-91fa568f-22fb-4cde-abc2-638836164c3d.png)

3.
```
ping www.google.com
```
![image](https://user-images.githubusercontent.com/77662970/193465747-fb157f6b-9616-4b0c-8a6f-6718534bc353.png)

4.
```
lynx fr.wikipedia.org
```

![image](https://user-images.githubusercontent.com/77662970/193466205-c5cd7e38-731b-48b0-9609-60b17a293bd7.png)

# Exercice 6

1.

![image](https://user-images.githubusercontent.com/77662970/193466627-6536d431-29e8-4294-80f3-12d5c0fef3c3.png)

2.

![image](https://user-images.githubusercontent.com/77662970/193466696-5feac532-e6cb-4c16-bd37-e151d105d58f.png)


![image](https://user-images.githubusercontent.com/77662970/193467031-f63dfdf2-c08c-479f-ad2f-202376fbb635.png)

3.

![image](https://user-images.githubusercontent.com/77662970/193470006-f0e1796b-f885-4c95-86ae-4d5c1f55ba36.png)


![image](https://user-images.githubusercontent.com/77662970/193469958-cb529e6d-2a2d-49f9-96b5-b8f83083bfa5.png)

4.
 
 ![image](https://user-images.githubusercontent.com/77662970/193470154-afbe5685-4242-4113-9bf0-6537f5227401.png)


![image](https://user-images.githubusercontent.com/77662970/193470164-ac6bc08f-1ee1-4e15-954f-22b82620df86.png)

5.

![image](https://user-images.githubusercontent.com/77662970/193470616-6d7e1bfd-0221-4911-9ff4-9f9dd7bef264.png)

![image](https://user-images.githubusercontent.com/77662970/193470497-fa7835d7-cbfc-4e7c-aef2-1dee03180dc5.png)

