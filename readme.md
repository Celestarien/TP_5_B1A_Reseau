# B1 Réseau 2018 - TP5

## TP 5 - Premier pas dans le monde Cisco

## I. Préparation du lab

### 1. Préparation VMs

1) Création d'un nouveau host-only :  
* On doit pour cela aller sur la fenêtre de gestion VirtualBox puis faire une clic droit ```« Configuration »``` sur une machine virtuelle, on se rendra ensuite dans la partie ```« Réseau »```. On pourra alors choisir un mode d'accès ```« réseau privé hôte »```.
* Par la suite, allez dans les paramètres de VirtualBox, ensuite dans l'onglet ```« Réseaux »``` puis ```« Serveur DHCP »``` et activez le.

2) Création des VMs

* On va juste cloner trois VMs depuis le patron du TP précédent :

    * server1.tp5.b1 est dans net1 et porte l'IP 10.5.1.10/24
    * client1.tp5.b1 est dans net2 et porte l'IP 10.5.2.10/24
    * client2.tp5.b1 est dans net2 et porte l'IP 10.5.2.11/24

* Ajoutez aux trois VMs une interface host-only en deuxième carte dans le host-only précédemment créé

3) Vous clonez juste les VMs, vous ne les allumez pas.

* Ensuite allez dans GNS3 : ```Edit > Preferences > VirtualBox VMs``` et vous ajoutez les trois VMs.

4) Configuration des réseaux dans GNS3

* Sur les 3 VMs :
    * Clic-droit > Configure > Network
    * Mettez 2 cartes réseau


### 2. Préparation Routeurs Cisco

Sur le réseau ```net12```, il n'y aura que 2 routeurs donc il nous faut seulement 2 adresses IP disponibles (adresse de réseau et de broadcast exclu), il nous faut donc une adresse IP se terminant par ```/30```. Nous pouvons donc prendre par exemple l'adresse IP suivante : ```10.5.12.8/30```.


| Machine       |     net1        |   net2     |    net12    
| ------------- |   ------------- | --------- | --------- 
|client1.tp5.b1 |        X        |  10.5.2.10 |     X      
|client2.tp5.b1 |        X        |  10.5.2.11 | 10.5.12.10 
|router1.tp5.b1 |   10.5.1.254    |      X     | 10.5.12.9 
|router2.tp5.b1 |        X        | 10.5.2.254 |     X      
|server1.tp5.b1 |    10.5.1.10    |      X     |     X      

## II. Lancement et configuration du lab

* Définition :
    * IP statique : Une adresse ```IP statique``` est une adresse IP qui est attribuée de manière permanente à une machine.
    * Nom de domaine : Un nom de domaine est un identifiant de domaine internet.

* Sur router1.tp5.b1 :
    * Entrez la commande : ```« conf t »```
    * Puis celle-ci : ```« ip route <NETWORK_2_ADDRESS> <MASK> <GATEWAY_IP> »```
    * Et enfin pour fermer : ```« exit »```
    * Vérifier les modifications avec : ```« show ip route »```

* Sur router2.tp5.b1 :
    * Entrez la commande : ```« conf t »```
    * Puis celle-ci : ```« ip route <NETWORK_1_ADDRESS> <MASK> <GATEWAY_IP> »```
    * Et enfin pour fermer : ```« exit »```
    * Vérifier les modifications avec : ```« sshow ip route »```

* Sur server1.tp5.b1 :
    * Entrez la commande : ```« conf t »```
    * Puis celle-ci : ```« ip route <NETWORK_2_ADDRESS> <MASK> <GATEWAY_IP> »```
    * Fermer avec : ```« exit »```
    * Vérifier les modifications avec : ```« sshow ip route »```
    * Et modifier fichier host (client1.tp5.b1 et client2.tp5.b1) avec : ```« <IP_ADDRESS>  <NOM> <NOM>.<X> »```

* Sur client1.tp5.b1 :
    * Entrez la commande : ```« conf t »```
    * Puis celle-ci : ```« ip route <NETWORK_1_ADDRESS> <MASK> <GATEWAY_IP> »```
    * Fermer avec : ```« exit »```
    * Vérifier les modifications avec : ```« sshow ip route »```
    * Et modifier fichier host (server1.tp5.b1 et client2.tp5.b1) avec : ```« <IP_ADDRESS>  <NOM> <NOM>.<X> »```

* Sur client2.tp5.b1 :
    * Entrez la commande : ```« conf t »```
    * Puis celle-ci : ```« ip route <NETWORK_1_ADDRESS> <MASK> <GATEWAY_IP> »```
    * Fermer avec : ```« exit »```
    * Vérifier les modifications avec : ```« sshow ip route »```
    * Et modifier fichier host (server1.tp5.b1, client1.tp5.b1) avec : ```« <IP_ADDRESS>  <NOM> <NOM>.<X> »```

* Sur router1.tp5.b1 :
    * Entrez la commande : ```« conf t »```
    * Puis celle-ci : ```« ip route <NETWORK_1_ADDRESS> <MASK> <GATEWAY_IP> »```
    * Et enfin pour fermer : ```« exit »```
    * Vérifier les modifications avec : ```« show ip route »```

Tester :  
Normalement, Client 1 & 2 peuvent :
```« ping server1.tp5.b1 »``` sans soucis.  
Ainsi que server1.tp5.b1 peut : ```« ping Client1 »``` & ```« ping Client2 »```

## III. DHCP

### 1. Mise en place du serveur DHCP

1) Renommer la machine
Sur GNS3, double-cliquez sur le nom de la machine et renommez la par dhcp-net2.tp5.b1

2) Installer le serveur DHCP :

* Éteindre la VM dans GNS3
* Ouvrir VirtualBox
* Ajouter une carte NAT à la machine virtuelle
* Démarrer la VM dans VirtualBox
* Allumer la carte NAT
* Utilisez la commande : ```« sudo yum install -y dhcp »```
* Éteindre à nouveau la machine virtuelle

3) Rallumer la VM

4) Configuration du serveur DHCP :
* Utilisez la commande :  ```« sudo nano /etc/dhcp/dhcpd.conf »```
* Cliquez [ici](https://github.com/It4lik/B1-Reseau-2018/blob/master/tp/5/dhcp/dhcpd.conf) pour savoir ce que vous avez à mettre dans ce fichier.