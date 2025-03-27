# Variables Enregistrées

## Exercice 

Voici le playbook ```kernel.yml``` :

```yaml
---
- name: Afficher les informations du noyau
  hosts: testing
  gather_facts: false
  tasks:
    - name: Exécuter la commande uname
      command: uname -a
      register: uname_cmd
      changed_when: false

    - name: Afficher le résultat avec msg
      debug:
        msg: "{{ uname_cmd.stdout_lines }}"
```

Voici la version avec le paramètre var du module debug

```yaml
---
- name: Afficher les informations du noyau
  hosts: testing
  gather_facts: false
  tasks:
    - name: Exécuter la commande uname
      command: uname -a
      register: uname_cmd
      changed_when: false

    - name: Afficher le résultat avec var
      debug:
        var: uname_cmd.stdout_lines
```

Voici le playbook ```packages.yml``` :

```yaml
---
- name: Compter les paquets RPM par hôte
  hosts: rocky,suse
  gather_facts: false
  tasks:
    - name: Compter les paquets RPM
      shell: rpm -qa | wc -l
      register: rpm_count
      changed_when: false

    - name: Afficher le résultat par hôte
      debug:
        msg: "Hôte {{ inventory_hostname }} : {{ rpm_count.stdout }} paquets RPM"
```

<br><br>

*Après avoir atteint une maitrise avancée d'ansible*

*La marmotte a transcandé l'univers*

<img src="https://github.com/user-attachments/assets/742f21ee-165b-4d6e-8e74-31a1adb13e04" alt="bowober-removebg-preview" width="25%" height="auto">
