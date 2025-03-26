# Variables

## Exercice

Voici le playbook ```myvars1.yml``` qui affiche ma voiture et moto préférée :

```
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

```
ansible-playbook myvars1.yml -e mycar=Honda
ansible-playbook myvars1.yml -e mycar=Honda -e mybike=Kawasaki
```

Voici le playbook ```myvars2``` qui utilise "set_fact" pour définir les variables :

```
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
