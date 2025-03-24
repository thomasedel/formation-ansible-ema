# Configuration

## Exercice

Après avoir lancé les VMs de l'atelier 6, faire ```vagrant ssh control```

Modifier le ```/etc/hosts``` pour ajouter les hosts :

```console
192.168.56.10 control
192.168.56.20 target01
192.168.56.30 target02
192.168.56.40 target03 	
```
On génère une paire de clé ssh avec : ```ssh-keygen```

On fait la commande : ```ssh-copy-id target0X``` sur les targets 1 à 3.

On fait ```sudo apt update```

Pour installer ansible utiliser la commande : ```sudo apt install ansible```

Pour vérifier l'installation d'ansible on fait ```ansible --version``` et on constate que la version installé est 2.10.8.

On test un premier ping ansible sur les 3 machines avec la commande : ```ansible all -i target01,target02,target03 -m ping```

![image](https://github.com/user-attachments/assets/48ff780a-0c79-4709-8732-5f8e917c76da)

On constate que les 3 pings sont success.

On créer le dossier monprojet avec la commande : ```mkdir monprojet```

Ensuite on utilise la commande ```touch ansible.cfg``` pour créer le fichier vide ansible.cfg

Pour vérifier si le fichier de config est pris en compte on fait ```ansible --version```

![image](https://github.com/user-attachments/assets/f62ba687-1b7d-4d04-87bf-b0730e796be3)


On créer un inventaire et on active les logs en ajoutant les lignes suivantes dans le fichier ```ansible.cfg``` :

```console
[defaults]
inventory = ./hosts
log_path = ~/logs/ansible.log
```

On créer le fichier ``hosts``` avec les lignes suivantes, au passage on défini l'utilisateur à utiliser : 

```console
[testlab]
target01
target02
target03

[testlab:vars]
ansible_user=vagrant
```

On fait ```ansible all -m ping``` :

Pour définir l'élévation on ajoute à la fin du fichier ```hosts``` la ligne ```ansible_become=yes```

![image](https://github.com/user-attachments/assets/a54ea5b1-cd70-4ea2-86f3-2484d04d1687)

On affiche la première ligne du fichier ```/etc/shadow``` des hosts avec la commande : ```ansible all -a "head -n 1 /etc/shadow"```

![image](https://github.com/user-attachments/assets/603a47e6-b5e5-472e-980c-0f5e0c79d8eb)


On quitte le ssh avec ```exit``` et on détruit les VMs avec ```vagrant destroy -f```
