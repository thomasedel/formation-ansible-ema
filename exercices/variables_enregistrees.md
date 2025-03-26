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
