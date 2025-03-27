# Variables

## Exercice

Voici le playbook ```myvars1.yml``` qui affiche ma voiture et moto préférée :

```yaml
---
- hosts: testing
  gather_facts: false

  vars:
    mycar: Toyota
    mybike: Yamaha

  tasks:
  - debug:
      msg: "Ma voiture préférée est {{ mycar }} et ma moto préférée est {{ mybike }}"

```

En utilisant les extra vars, on remplace une marque puis deux à la fois :

```shell
ansible-playbook myvars1.yml -e mycar=Honda
ansible-playbook myvars1.yml -e mycar=Honda -e mybike=Kawasaki
```

Voici le playbook ```myvars2``` qui utilise "set_fact" pour définir les variables :

```yaml
---
- hosts: testing
  gather_facts: false

  tasks:
  - name: Définir les variables
    set_fact:
      mycar: Toyota
      mybike: Yamaha

  - debug:
      msg: "Ma voiture préférée est {{ mycar }} et ma moto préférée est {{ mybike }}"

```

Comme précédement on utilise les extra vars : 
```
ansible-playbook myvars2.yml -e mycar=Honda
ansible-playbook myvars2.yml -e mycar=Honda -e mybike=Kawasaki
```

Pour le playbook "myvars3.yml", on commence par créer le dossier group_vars à la racine du projet.

Dans ce dossier, on créer le fichier ```testing.yml``` avec le contenu suivant :

````yaml
--- # group_vars/testing.yml
mycar: VW
mybike: BMW
````

Maitenant on créer le fichier ```myvars3.yml``` avec le contenu suivant : 

```yaml
---
- hosts: testing
  gather_facts: false

  tasks:
  - debug:
      msg: "Ma voiture préférée est {{ mycar }} et ma moto préférée est {{ mybike }}"
```

On lance le playbook avec ```ansible-playbook myvars3.yml```

On constate que les variables défini dans ```testing.yml``` sont bien utilisés.


Afin de remplacer VW et BMW par Mercedes et Honda sur l'hôte target02 on créer le dossier ```host_vars``` à la racine du projet : ```mkdir host_vars```.

Dans ce dossier on créer le fichier ```testing02.yaml``` en y ajoutant le contenu suivant :

```yaml
---
mycar: Mercedes
mybike: Honda
```

Maitenant quand on relance le playbook ```myvars3.yml``` on constate que pour le serveur testing02, les variables sont différentes et on a :

```shell
TASK [debug] *******************************************************************
ok: [target01] => {
    "msg": "Ma voiture préférée est VW et ma moto préférée est BMW"
}
ok: [target02] => {
    "msg": "Ma voiture préférée est Mercedes et ma moto préférée est Honda"
}
ok: [target03] => {
    "msg": "Ma voiture préférée est VW et ma moto préférée est BMW"
}
```

Voici le playbook ```display_user.yml``` :

```yaml
---
- hosts: target01
  gather_facts: false

  vars_prompt:
  - name: user
    prompt: "Entrez le nom d'utilisateur"
    default: microlinux
    private: false
  - name: password
    prompt: "Entrez le mot de passe (secret)"
    default: yatahongaga
    private: true

  tasks:
  - debug:
      msg: "Utilisateur : {{ user }}, Mot de passe : {{ password }}"
```

On le lance avec ```ansible-playbook display_user.yml```

On constate que par defaut, les credentials sont bien microlinux:yatahongaga.

On peut les remplacer en spécifiant un utilisateurr et un mot de passe.

Lors de la saisie du mot de passe, la saisie est cachée.

<br>
<br>

*Les marmottes n'ont rien compris au playbook*

*Pendant que vous attendez la fin du ```vagrant up``` elles ont volé votre moto et votre voiture*

<img src="https://github.com/user-attachments/assets/0143c638-c299-458c-a004-75ed3600071d" alt="bowober-removebg-preview" width="300" height="auto">
