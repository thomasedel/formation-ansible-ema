# Installation

## Exercice 1

Se rendre dans le dossier atelier-01 : ```cd /home/ema/formation-ansible/atelier-01/```

Lancer la vm ubuntu : ```vagrant up ubuntu```

Se connecter en SSH à la VM : ```vagrant ssh ubuntu```

Rafraîchir les paquets : ```sudo apt update```

Recherche du paquet ansible : ```apt-cache search --names-only ansible```

Installer le paquet ansible : ```apt install ansible```

Vérifier si l'installation s'est bien déroulé : ```ansible --version```

On constate que la version est 2.10.8 :

![image](https://github.com/user-attachments/assets/0277d7ec-b0f7-4083-bc20-7ffd0ad1542f)

Sur la machine hôte executer la commande suivante : ```vagrant destroy ubuntu```

## Exercice 2

Se rendre dans le dossier atelier-01 : ```cd /home/ema/formation-ansible/atelier-01/```

Lancer la vm ubuntu : ```vagrant up ubuntu```

Se connecter en SSH à la VM : ```vagrant ssh ubuntu```

Ajouter le PPA : ```sudo apt-add-repository ppa:ansible/ansible```

Rafraîchir les paquets : ```sudo apt update```

Recherche du paquet ansible : ```apt-cache search --names-only ansible```

Installer le paquet ansible : ```apt install ansible```

Vérifier si l'installation s'est bien déroulé : ```ansible --version```

On constate que la varsion est 2.17.9 :

![image](https://github.com/user-attachments/assets/96ab50ab-f16f-402d-9fdb-ba8bc73f0c2c)

## Exercice 3


