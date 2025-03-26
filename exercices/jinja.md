# Jinja

## Exercice

Dans un premier temps, on créer le dossier ```templates``` à la racine et on y ajoute dedans le fichier ```chrony.conf.j2``` contenant les lignes suivantes : 

```console
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
# Ce playbook installe un fichier de configuration personnalisé pour Chrony sur les cibles.
# Le fichier sera installé à /etc/chrony.conf ou /etc/chrony/chrony.conf selon la distribution.

- name: Install and configure Chrony
  hosts: testing
  become: yes

  vars:
    chrony_package: chrony
    chrony_service: "{{ 'chrony' if ansible_os_family == 'Debian' else 'chronyd' }}"
    chrony_confdir: "{{ '/etc/chrony.conf' if ansible_os_family == 'Debian' else '/etc/chrony/chrony.conf' }}"

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

    - name: Ensure the /etc/chrony directory exists on non-Debian systems
      file:
        path: /etc/chrony
        state: directory
      when: ansible_distribution in ["Rocky", "openSUSE Leap"]

    - name: Configure Chrony with a custom configuration file
      template:
        src: templates/chrony.conf.j2
        dest: "{{ chrony_confdir }}"
      notify: Restart Chrony

    - name: Enable and start Chrony service
      service:
        name: "{{ chrony_service }}"
        state: started
        enabled: yes

  handlers:
    - name: Restart Chrony
      service:
        name: "{{ chrony_service }}"
        state: restarted
```

On lance le playbook avec ```ansible-playbook chrony.yml```

On se connecte en ssh au serveur debian pour voir si la config a été appliqué :

```shell
vagrant@debian:~$ cat /etc/chrony.conf 
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

On fait pareil pour le serveur rocky et on voit que la config a aussi été appliqué :

```shell
[vagrant@ansible ema]$ ssh rocky
Last login: Wed Mar 26 11:22:18 2025 from 192.168.56.10
[vagrant@rocky ~]$ cat /etc/chrony/chrony.conf 
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

<br><br>
*La marmotte dyslexique a lu Ninja et non Jinja*

*Elle est très déçu, elle vous provoque en duel pour laver son honneur*

<img src="https://github.com/user-attachments/assets/a1ef7811-2ccf-4da8-a423-7cf017b82f72" alt="bowober-removebg-preview" width="300" height="auto">
