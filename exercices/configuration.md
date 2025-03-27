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

```shell
vagrant@control:~$ ansible all -i target01,target02,target03 -m ping
target02 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
target03 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
target01 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
```

On constate que les 3 pings fonctionnent.

On créer le dossier monprojet avec la commande : ```mkdir monprojet```

Ensuite on utilise la commande ```touch ansible.cfg``` pour créer le fichier vide ansible.cfg

Pour vérifier si le fichier de config est pris en compte on fait ```ansible --version```

```shell
vagrant@control:~/monprojet$ ansible --version
ansible 2.10.8
  config file = /home/vagrant/monprojet/ansible.cfg
  configured module search path = ['/home/vagrant/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules']
  ansible python module location = /usr/lib/python3/dist-packages/ansible
  executable location = /usr/bin/ansible
  python version = 3.10.12 (main, Jan 17 2025, 14:35:34) [GCC 11.4.0]
```


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

```shell
vagrant@control:~/monprojet$ ansible all -m ping
[WARNING]: log file at /home/vagrant/logs/ansible.log is not writeable and we cannot create it, aborting

target03 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
target02 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
target01 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
```

Pour définir l'élévation on ajoute à la fin du fichier ```hosts``` la ligne ```ansible_become=yes```

On affiche la première ligne du fichier ```/etc/shadow``` des hosts avec la commande : ```ansible all -a "head -n 1 /etc/shadow"```

```shell
vagrant@control:~/monprojet$ ansible all -a "head -n 1 /etc/shadow"
[WARNING]: log file at /home/vagrant/logs/ansible.log is not writeable and we cannot create it, aborting

target01 | CHANGED | rc=0 >>
root:*:19977:0:99999:7:::
target03 | CHANGED | rc=0 >>
root:*:19977:0:99999:7:::
target02 | CHANGED | rc=0 >>
root:*:19977:0:99999:7:::
```

On quitte le ssh avec ```exit``` et on détruit les VMs avec ```vagrant destroy -f```

<br>

*La marmotte ne s'est pas reveillé pour le cours ...*

*Elle n'a pas signalé sont retard à l'administration*
