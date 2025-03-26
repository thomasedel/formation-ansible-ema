# Cibles Hétérogenes

## Exercice

Playbook ```chrony-01.yml``` aka la méthode gros sabots :

```yaml
---
- hosts: testing

  tasks:
    - name: Update package information on Debian/Ubuntu
      apt:
        update_cache: true
        cache_valid_time: 3600
      when: ansible_os_family == "Debian"

    - name: Install Chrony on Debian/Ubuntu
      apt:
        name: chrony
      when: ansible_os_family == "Debian"

    - name: Install Chrony on Rocky Linux
      dnf:
        name: chrony
      when: ansible_distribution == "Rocky"

    - name: Install Chrony on SUSE Linux
      zypper:
        name: chrony
      when: ansible_distribution == "openSUSE Leap"

    - name: Configure Chrony on Debian/Ubuntu
      copy:
        content: |
          # chrony.conf
          server 0.fr.pool.ntp.org iburst
          server 1.fr.pool.ntp.org iburst
          server 2.fr.pool.ntp.org iburst
          server 3.fr.pool.ntp.org iburst
          driftfile /var/lib/chrony/drift
          makestep 1.0 3
          rtcsync
          logdir /var/log/chrony
        dest: /etc/chrony.conf
      when: ansible_os_family == "Debian"

    - name: Configure Chrony on Rocky Linux
      copy:
        content: |
          # chrony.conf
          server 0.fr.pool.ntp.org iburst
          server 1.fr.pool.ntp.org iburst
          server 2.fr.pool.ntp.org iburst
          server 3.fr.pool.ntp.org iburst
          driftfile /var/lib/chrony/drift
          makestep 1.0 3
          rtcsync
          logdir /var/log/chrony
        dest: /etc/chrony.conf
      when: ansible_distribution == "Rocky"

    - name: Configure Chrony on SUSE Linux
      copy:
        content: |
          # chrony.conf
          server 0.fr.pool.ntp.org iburst
          server 1.fr.pool.ntp.org iburst
          server 2.fr.pool.ntp.org iburst
          server 3.fr.pool.ntp.org iburst
          driftfile /var/lib/chrony/drift
          makestep 1.0 3
          rtcsync
          logdir /var/log/chrony
        dest: /etc/chrony.conf
      when: ansible_distribution == "openSUSE Leap"

    - name: Enable and start Chrony on Debian/Ubuntu
      service:
        name: chrony
        state: started
        enabled: yes
      when: ansible_os_family == "Debian"

    - name: Enable and start Chrony on Rocky Linux
      service:
        name: chronyd
        state: started
        enabled: yes
      when: ansible_distribution == "Rocky"

    - name: Enable and start Chrony on SUSE Linux
      service:
        name: chronyd
        state: started
        enabled: yes
      when: ansible_distribution == "openSUSE Leap"
```

On lance le playbook avec la commande ```ansible-playbook chrony-01.yml```

On constate la bonne detection des os avec :

```shell
TASK [Define distribution-specific variables] *******************************************************************
skipping: [rocky]
ok: [debian]
skipping: [suse]
ok: [ubuntu]

TASK [Define distribution-specific variables for Rocky Linux] ***************************************************
ok: [rocky]
skipping: [debian]
skipping: [suse]
skipping: [ubuntu]

TASK [Define distribution-specific variables for SUSE Linux] ****************************************************
skipping: [rocky]
skipping: [debian]
ok: [suse]
skipping: [ubuntu]
```

Voici le playbook ```chrony-02.yml``` :

On lance le playbook avec la commande ```ansible-playbook chrony-02.yml```

```yaml
---
- name: Configure Chrony with native package managers
  hosts: testing
  become: yes

  tasks:
  - name: Update package information on Debian/Ubuntu
    apt:
      update_cache: true
      cache_valid_time: 3600
    when: ansible_os_family == "Debian"

  - name: Install Chrony
    package:
      name: chrony
    when: ansible_os_family in ["Debian", "RedHat", "Suse"]

  - name: Configure Chrony with static config on Debian/Ubuntu
    copy:
      content: |
        # chrony.conf
        server 0.fr.pool.ntp.org iburst
        server 1.fr.pool.ntp.org iburst
        server 2.fr.pool.ntp.org iburst
        server 3.fr.pool.ntp.org iburst
        driftfile /var/lib/chrony/drift
        makestep 1.0 3
        rtcsync
        logdir /var/log/chrony
      dest: /etc/chrony.conf
    notify:
      - Restart Chrony (Debian)
      - Restart Chronyd (RHEL/SUSE)
    when: ansible_os_family == "Debian"

  - name: Configure Chrony with static config on Rocky Linux
    copy:
      content: |
        # chrony.conf
        server 0.fr.pool.ntp.org iburst
        server 1.fr.pool.ntp.org iburst
        server 2.fr.pool.ntp.org iburst
        server 3.fr.pool.ntp.org iburst
        driftfile /var/lib/chrony/drift
        makestep 1.0 3
        rtcsync
        logdir /var/log/chrony
      dest: /etc/chrony.conf
    notify:
      - Restart Chrony (Debian)
      - Restart Chronyd (RHEL/SUSE)
    when: ansible_distribution == "Rocky"

  - name: Configure Chrony with static config on SUSE Linux
    copy:
      content: |
        # chrony.conf
        server 0.fr.pool.ntp.org iburst
        server 1.fr.pool.ntp.org iburst
        server 2.fr.pool.ntp.org iburst
        server 3.fr.pool.ntp.org iburst
        driftfile /var/lib/chrony/drift
        makestep 1.0 3
        rtcsync
        logdir /var/log/chrony
      dest: /etc/chrony.conf
    notify:
      - Restart Chrony (Debian)
      - Restart Chronyd (RHEL/SUSE)
    when: ansible_distribution == "openSUSE Leap"

  - name: Enable and start Chrony service
    service:
      name: "{{ 'chrony' if ansible_os_family == 'Debian' else 'chronyd' }}"
      state: started
      enabled: yes
    when: ansible_os_family in ["Debian", "RedHat", "Suse"]

  handlers:
  - name: Restart Chrony (Debian)
    service:
      name: chrony
      state: restarted
    when: ansible_os_family == "Debian"

  - name: Restart Chronyd (RHEL/SUSE)
    service:
      name: chronyd
      state: restarted
    when: ansible_os_family in ["RedHat", "Suse"]
```

<br>
<br>

*Cette marmotte dodu se demande ce qu'est ansible*

*Elle retourne dans son terrier*

<img src="https://github.com/user-attachments/assets/c3f92569-1c16-4b34-bec5-758b8eadfe72" alt="bowober-removebg-preview" width="25%" height="auto">
