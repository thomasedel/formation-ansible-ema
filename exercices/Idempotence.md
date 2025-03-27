# Idempotence

## Exercice

Pour installer les 3 paquets lancer :  

```shell
ansible all -m package -a "name=tree"
ansible all -m package -a "name=git"
ansible all -m package -a "name=nmap"
```

Pour désinstaller les 3 paquets lancer : 

```shell
ansible all -m package -a "name=tree state=absent"
ansible all -m package -a "name=git state=absent"
ansible all -m package -a "name=nmap state=absent"
```

Pour copier le fichier fstab lancer : ```ansible all -m copy -a "src=/etc/fstab dest=/tmp/test3.txt"```

Pour supprimer le fichier lancer : ```ansible all -m file -a "path=/tmp/test3.txt state=absent"```

On utilise la commande ```ansible all -m shell -a "df -h /"``` pour  affichez l’espace utilisé par la partition principale sur tous les Target Hosts.

On constate que la couleur est en jaune. Cela est du au fait que le module shell ne permet pas à ansible de savoir si quelque chose a changé à la suite de la commande.

<br><br>

*Après avoir entendu parler d'idempotence, la marmotte est devenue prof de maths*

<img src="https://github.com/user-attachments/assets/177076c9-3e5e-42a4-96d8-f25d0c844dd5" alt="bowober-removebg-preview" width="25%" height="auto">
