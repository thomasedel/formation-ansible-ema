# Authentification

## Exercice

On se déplace dans le dossier avec : ```cd ~/formation-ansible/atelier-03```

On lance les VMs avec : ```vagrant up```

On se connecter en SSH à la VM control avec : ```vagrant ssh control```

On vérifie que ansible est installé avec : ```type ansible```

Modifier le ```/etc/hosts``` pour ajouter les hosts :

```bash
192.168.56.10 control
192.168.56.20 target01
192.168.56.30 target02
192.168.56.40 target03 	
```
On génère une paire de clé ssh avec : ```ssh-keygen```

On fait la commande : ```ssh-copy-id target0X``` sur les targets 1 à 3.

On test un ping ansible sur les 3 machines avec la commande : ```ansible all -i target01,target02,target03 -m ping```

On constate que les pings fonctionnent :

```bash
vagrant@control:~$ ansible all -i target01,target02,target03 -m ping
target01 | SUCCESS => {
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
target03 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
```
<br>

*La marmotte à volé votre clé privé*

*Elle peut maitenant se connecter à vos machines pour miner du bitcoin*

<img src="https://github.com/user-attachments/assets/1917d0c1-46c7-43f7-89fd-a827cfcd2cc0" alt="bowober-removebg-preview" width="25%" height="auto">
