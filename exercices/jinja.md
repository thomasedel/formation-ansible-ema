# Jinja

## Exercice

Dans un premier temps, on créer le dossier ```templates``` à la racine et on y ajoute le fichier ```chrony.conf``` contenant les lignes suivantes : 

```console
# chrony.conf
server 0.fr.pool.ntp.org iburst
server 1.fr.pool.ntp.org iburst
server 2.fr.pool.ntp.org iburst
server 3.fr.pool.ntp.org iburst
driftfile /var/lib/chrony/drift
makestep 1.0 3
rtcsync
logdir /var/log/chrony
```

Voici le playbook ```chrony.yml``` :

```yaml
---
- name: Configure Chrony with native package managers
  hosts: testing
  become: yes

  vars:
    chrony_package: chrony
    chrony_service: "{{ 'chrony' if ansible_os_family == 'Debian' else 'chronyd' }}"
    chrony_confdir: /etc/chrony.conf

  tasks:
    - name: Update package information on Debian/Ubuntu
      apt:
        update_cache: true
        cache_valid_time: 3600
      when: ansible_os_family == "Debian"

    - name: Install Chrony on Debian/Ubuntu
      package:
        name: "{{ chrony_package }}"
      when: ansible_os_family == "Debian"

    - name: Install Chrony on Rocky Linux
      package:
        name: "{{ chrony_package }}"
      when: ansible_distribution == "Rocky"

    - name: Install Chrony on SUSE Linux
      package:
        name: "{{ chrony_package }}"
      when: ansible_distribution == "openSUSE Leap"

    - name: Configure Chrony on Debian/Ubuntu
      template:
        src: templates/chrony.conf.j2
        dest: "{{ chrony_confdir }}"
      when: ansible_os_family == "Debian"

    - name: Configure Chrony on Rocky Linux
      template:
        src: templates/chrony.conf.j2
        dest: "{{ chrony_confdir }}"
      when: ansible_distribution == "Rocky"

    - name: Configure Chrony on SUSE Linux
      template:
        src: templates/chrony.conf.j2
        dest: "{{ chrony_confdir }}"
      when: ansible_distribution == "openSUSE Leap"

    - name: Enable and start Chrony on Debian/Ubuntu
      service:
        name: "{{ chrony_service }}"
        state: started
        enabled: yes
      when: ansible_os_family == "Debian"

    - name: Enable and start Chrony on Rocky Linux
      service:
        name: "{{ chrony_service }}"
        state: started
        enabled: yes
      when: ansible_distribution == "Rocky"

    - name: Enable and start Chrony on SUSE Linux
      service:
        name: "{{ chrony_service }}"
        state: started
        enabled: yes
      when: ansible_distribution == "openSUSE Leap"

```

<br><br>
*La marmotte dyslexique a lu Ninja et non Jinja*

*Elle est très déçu, elle vous provoque en duel pour laver son honneur*

<img src="https://github.com/user-attachments/assets/a1ef7811-2ccf-4da8-a423-7cf017b82f72" alt="bowober-removebg-preview" width="300" height="auto">
