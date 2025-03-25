# Idempotence

## Exercice

Pour installer les 3 paquets lancer :  ```ansible all -m package -a "name=tree,git,nmap"```

Pour désinstaller les 3 paquets lancer : ```ansible all -m package -a "name=tree,git,nmap state=absent"```

Pour copier le fichier fstab lancer : ```ansible all -m copy -a "src=/etc/fstab dest=/tmp/test3.txt"```

Pour supprimer le fichier lancer : ```ansible all -m file -a "path=/tmp/test3.txt state=absent"```

On utilise la commande ```ansible all -m shell -a "df -h /"``` pour  affichez l’espace utilisé par la partition principale sur tous les Target Hosts.

On constate que la couleur est en jaune. Cela est du au fait que le module shell ne permet pas à ansible de savoir si quelque chose a changé à la suite de la commande.
