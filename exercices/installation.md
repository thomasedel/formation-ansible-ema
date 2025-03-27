# Installation

## Exercice 1

Se rendre dans le dossier atelier-01 : ```cd /home/ema/formation-ansible/atelier-01/```

Lancer la vm ubuntu : ```vagrant up ubuntu```

Se connecter en SSH à la VM : ```vagrant ssh ubuntu```

Rafraîchir les paquets : ```sudo apt update```

Recherche du paquet ansible : ```apt-cache search --names-only ansible```

Installer le paquet ansible : ```sudo apt install ansible```

Vérifier si l'installation s'est bien déroulé :

On constate avec ```ansible --version``` que la version est 2.10.8 :

```console
vagrant@ubuntu:~$ ansible --version
ansible 2.10.8
  config file = None
  configured module search path = ['/home/vagrant/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules']
  ansible python module location = /usr/lib/python3/dist-packages/ansible
  executable location = /usr/bin/ansible
  python version = 3.10.12 (main, Jan 17 2025, 14:35:34) [GCC 11.4.0]
```

Sur la machine hôte executer la commande suivante : ```vagrant destroy ubuntu```

## Exercice 2

Se rendre dans le dossier atelier-01 : ```cd /home/ema/formation-ansible/atelier-01/```

Lancer la vm ubuntu : ```vagrant up ubuntu```

Se connecter en SSH à la VM : ```vagrant ssh ubuntu```

Ajouter le PPA : ```sudo apt-add-repository ppa:ansible/ansible```

Rafraîchir les paquets : ```sudo apt update```

Recherche du paquet ansible : ```apt-cache search --names-only ansible```

Installer le paquet ansible : ```apt install ansible```

Vérifier si l'installation s'est bien déroulée :

On constate avec ```ansible --version``` que la version d'ansible est 2.17.10:

```console
vagrant@ubuntu:~$ ansible --version
ansible [core 2.17.10]
  config file = /etc/ansible/ansible.cfg
  configured module search path = ['/home/vagrant/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules']
  ansible python module location = /usr/lib/python3/dist-packages/ansible
  ansible collection location = /home/vagrant/.ansible/collections:/usr/share/ansible/collections
  executable location = /usr/bin/ansible
  python version = 3.10.12 (main, Jan 17 2025, 14:35:34) [GCC 11.4.0] (/usr/bin/python3)
  jinja version = 3.0.3
  libyaml = True
```

## Exercice 3

Lancer une VM Rocky : ```vagrant up rocky```

Se connecter à la VM Rocky en SSH : ```vagrant ssh rocky```

Installer pip : ```sudo dnf install python3-pip```

Créer un venv : ```python3 -m venv ~/.venv/ansible```

Activer le venv : ```source ~/.venv/ansible/bin/activate```

Mettre à jour pip : ```pip install --upgrade pip```

Installer ansible via pip : ```pip install ansible```

Vérifier si l'installation s'est bien déroulée :

On constate avec ```ansible --version``` que la version d'ansible est 2.15.13 :

```console
(ansible) [vagrant@rocky ~]$ ansible --version
ansible [core 2.15.13]
  config file = None
  configured module search path = ['/home/vagrant/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules']
  ansible python module location = /home/vagrant/.venv/ansible/lib64/python3.9/site-packages/ansible
  ansible collection location = /home/vagrant/.ansible/collections:/usr/share/ansible/collections
  executable location = /home/vagrant/.venv/ansible/bin/ansible
  python version = 3.9.18 (main, Sep  7 2023, 00:00:00) [GCC 11.4.1 20230605 (Red Hat 11.4.1-2)] (/home/vagrant/.venv/ansible/bin/python3)
  jinja version = 3.1.6
  libyaml = True
```


