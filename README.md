# tp2-cs
# Implémentation d’attaques 
## my_ping.c
générer un paquet IP echo
```cpp
#cc -c my_ping.c  pour compiler
#cc my_ping.c –o myping  pour générer l’exécutable
#./myping 127.0.0.3 127.0.0.4 500
```
on utilise WireShark pour observer le résultat
![Capture d’écran 2021-12-15 171321](https://user-images.githubusercontent.com/85891554/148665145-56037a0f-bb84-4eee-8b6e-a0748db8c4a5.png)
## pingsur2frag.c
faire un ping avec une taille de 2000 octets sur le lien ethernet
```cpp
#cc -c pingfragments.c  pour compiler
#cc pingfragments.c –o pf  pour générer l’exécutable
#./pf 127.0.0.3 127.0.0.4 2000
```
on utilise WireShark pour observer différents champs des fragments IP
![Capture d’écran 2021-12-15 223506](https://user-images.githubusercontent.com/85891554/148665162-ecd0f98a-ac07-407d-9c88-d8e7cc702886.png)
![Capture d’écran 2021-12-15 223124](https://user-images.githubusercontent.com/85891554/148665177-ff62961a-6ffc-4309-9684-715f2faa3b0e.png)
![Capture d’écran 2021-12-15 224322](https://user-images.githubusercontent.com/85891554/148665179-df29fd77-6cdb-482d-a5d2-62764d10ed86.png)

