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

SS-1 =
172.16.0.0/24
8 + 8 + 7 + 0
11111111.11111111.00000000.00000000 = 
172.16.0.0/26
Adresse de la première machine = 172.16.0.1/26 Adresse Broadcast =

SS-2 = 
172.16.0.0/24
8 + 8 + 7 + 0
11111111.11111111.00000000.01000000 =
172.16.0.64/26
Adresse de la première machine = 172.16.0.65/26

SS-3 =
172.16.0.0/24
8 + 8 + 7 + 0
11111111.11111111.00000000.11000000 =
172.16.0.192/26
Adresse de la première machine = 172.16.0.193/26

SS-4 = 
172.16.0.0/24
8 + 8 + 7 + 0
11111111.11111111.00000001.11000000 =
172.16.1.192/26

SS-5 = 
172.16.0.0/24
8 + 8 + 7 + 0
11111111.11111111.00000001.000000 =
172.16.1.0/26

SS-6 = 
172.16.0.0/24
8 + 8 + 7 + 0
11111111.11111111.00000000.10000000 =
172.16.0.128/26

SS-7 = 
172.16.0.0/24
8 + 8 + 7 + 0
11111111.11111111.00000001.01000000 =
172.16.1.64/26

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
    ense224
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

