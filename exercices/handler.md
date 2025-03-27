# Handler

## Exercice

On se connecte sur la machine ansible avec : ```vagrant ssh ansible```



Voici le playbook à créer dans : 

```yaml
---
- name: Configure Chrony for NTP synchronization
  hosts: redhat
  become: yes

  tasks:
    - name: Install Chrony package
      dnf:
        name: chrony
        state: present

    - name: Configure Chrony servers
      blockinfile:
        path: /etc/chrony.conf
        block: |
          server 0.fr.pool.ntp.org iburst
          server 1.fr.pool.ntp.org iburst
          server 2.fr.pool.ntp.org iburst
          server 3.fr.pool.ntp.org iburst
          driftfile /var/lib/chrony/drift
          makestep 1.0 3
          rtcsync
          logdir /var/log/chrony
        state: present
        backup: yes
      notify: restart chronyd

    - name: Enable and start Chrony service
      service:
        name: chronyd
        state: started
        enabled: yes

  handlers:
    - name: restart chronyd
      service:
        name: chronyd
        state: restarted

```

On le lance avec : ```ansible-playbook chrony.yml```

Si on le relance on a le retour suivant :

```shell
PLAY RECAP *********************************************************************
target01                   : ok=4    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
target02                   : ok=4    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
target03                   : ok=4    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```

On constate qu'il n'y a aucun "changed" donc le script est idempotent car il n'a rien modifié la 2nd fois.

<br><br>

*Après avoir appris a utiliser chrony, Docteur Marmotte contrôle maintenant l'espace et le temps*

<img src="https://github.com/user-attachments/assets/7eaa3ef1-df25-44ba-8582-e7d6faba65bc" alt="bowober-removebg-preview" width="25%" height="auto">


