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
11111111.11111111.00000000.00000000
172.16.254.0/26

SS-2 = 
172.16.0.0/24
8 + 8 + 7 + 0
11111111.11111111.00000000.01000000
172.16.254.64/26

SS-3 =
172.16.0.0/24
8 + 8 + 7 + 0
11111111.11111111.00000000.11000000
172.16.254.192/26

SS-4 = 
172.16.0.0/24
8 + 8 + 7 + 0
11111111.11111111.00000001.11000000
172.16.255.192/26

SS-5 = 
172.16.0.0/24
8 + 8 + 7 + 0
11111111.11111111.00000001.000000
172.16.0.0/26

# Exercice 2. Préparation de l’environnement
1
2. C'est une boucle....
