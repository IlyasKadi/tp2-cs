# tp2-cs
# Implémentation d’attaques 
## my_ping.c
générer un paquet IP echo
```cpp
#cc -c my_ping.c  pour compiler
#cc my_ping.c –o myping  pour générer l’exécutable
#./myping 192.168.1.1 192.168.1.2 500
```
on utilise WireShark pour observer le résultat
![Capture d’écran 2021-12-15 171321](https://user-images.githubusercontent.com/85891554/148665145-56037a0f-bb84-4eee-8b6e-a0748db8c4a5.png)
## pingsur2frag.c
faire un ping avec une taille de 2000 octets sur le lien ethernet
```cpp
#cc -c pingfragments.c  pour compiler
#cc pingfragments.c –o pf  pour générer l’exécutable
#./pf 192.168.1.1 192.168.1.2 2000
```
on utilise WireShark pour observer les différents champs des fragments IP

![Capture d’écran 2021-12-15 223506](https://user-images.githubusercontent.com/85891554/148665162-ecd0f98a-ac07-407d-9c88-d8e7cc702886.png)

## pingfragments.c
compiler et générer l’exécutable
```cpp
#cc -c pingsur2frag.c  pour compiler
#cc pingsur2frag.c –o psf  pour générer l’exécutable
#./psf 192.168.1.1 192.168.1.2 2000
```
observer le resultat 

![Capture d’écran 2021-12-15 223124](https://user-images.githubusercontent.com/85891554/148665177-ff62961a-6ffc-4309-9684-715f2faa3b0e.png)

## demandeconntcp.c
compiler et générer l’exécutable
```cpp
#cc -c demandeconntcp.c  pour compiler
#cc demandeconntcp.c –o dct  pour générer l’exécutable
#./dct 192.168.1.1 192.168.1.2 2000
```
resultat : 

![Capture d’écran 2021-12-15 224322](https://user-images.githubusercontent.com/85891554/148665179-df29fd77-6cdb-482d-a5d2-62764d10ed86.png)

# Test de quelques outils d’attaques2
## DHCP starvation

L’attaquant inonde le serveur DHCP avec des messages DHCPREQUEST afin de réserver toutes les adresses IP disponibles sur le serveur DHCP. L’attaquant doit utiliser une nouvelle adresse MAC pour chaque requête 

## Manipulation :
```cpp
#tar –xvf dhcpstarv-0.2.1.tar.gz
```
![Capture d’écran 2021-12-20 143228 (2)](https://user-images.githubusercontent.com/85891554/148682658-4aa374db-60dd-4e24-af09-faf6207a4876.png)
```cpp
#cd dhcpstarv-0.2.1
#./configure
```
![Capture d’écran 2021-12-30 131121](https://user-images.githubusercontent.com/85891554/148682702-642c7f90-0ccf-4d79-bdc7-473fa0c3085f.png)
```cpp
#make
```
![Capture d’écran 2022-01-09 134601](https://user-images.githubusercontent.com/85891554/148682745-d222fb6c-a63f-479c-b578-031b1a8c1277.png)
```cpp
#make install 
```
![Capture d’écran 2022-01-09 134631](https://user-images.githubusercontent.com/85891554/148682750-00da8427-69b5-4046-ade8-6229e73eaa12.png)

Sur une autre machine, on va configurer le serveur DHCP
_ configuration du dhcp _

![Capture d’écran 2022-01-09 135330](https://user-images.githubusercontent.com/85891554/148683028-cbb5c56d-1a0e-49c3-9c7d-2fda018eac72.png)

on observe sur WireShark l'envoie des demandes dhcp (dhcp discover)

![Capture d’écran 2022-01-09 135533](https://user-images.githubusercontent.com/85891554/148683036-7fe8905c-ab02-4d9f-8370-9c0ecd4f64a5.png)

l'assignment des adresses ip

![Capture d’écran 2022-01-09 135433](https://user-images.githubusercontent.com/85891554/148683039-c7dfe30d-00f1-4ff3-ad4b-b6bf9d416c74.png)

# Attaque “Man In The Middle” basée sur l'attaque “ARP spoofing”
