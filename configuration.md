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


On créé un inventaire en ajoutant les lignes suivantes dans le fichier ```ansible.cfg``` :

```console
[hosts]
testing01
testing02
testing03
```
