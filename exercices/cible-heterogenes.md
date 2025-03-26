# Cibles Hétérogenes

## Exercice

Playbook ```chrony-01.yml``` aka la méthode gros sabots :

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
    template:
      src: templates/chrony.conf.j2
      dest: /etc/chrony.conf
    notify: Restart Chrony
    when: ansible_os_family == "Debian"

  - name: Configure Chrony on Rocky Linux
    template:
      src: templates/chrony.conf.j2
      dest: /etc/chrony.conf
    notify: Restart Chronyd
    when: ansible_distribution == "Rocky"

  - name: Configure Chrony on SUSE Linux
    template:
      src: templates/chrony.conf.j2
      dest: /etc/chrony.conf
    notify: Restart Chronyd
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

  handlers:
  - name: Restart Chrony
    service:
      name: chrony
      state: restarted
    when: ansible_os_family == "Debian"
  - name: Restart Chronyd
    service:
      name: chronyd
      state: restarted
    when: ansible_distribution in ["Rocky", "openSUSE Leap"]
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
- name: Configure Chrony with generic package manager
  hosts: testing
  become: yes

  tasks:
  - name: Define distribution-specific variables
    set_fact:
      chrony_package: chrony
      chrony_service: chrony
      chrony_confdir: /etc
    when: ansible_os_family == "Debian"

  - name: Define distribution-specific variables for Rocky Linux
    set_fact:
      chrony_package: chrony
      chrony_service: chronyd
      chrony_confdir: /etc
    when: ansible_distribution == "Rocky"

  - name: Define distribution-specific variables for SUSE Linux
    set_fact:
      chrony_package: chrony
      chrony_service: chronyd
      chrony_confdir: /etc
    when: ansible_distribution == "openSUSE Leap"

  - name: Install Chrony
    package:
      name: "{{ chrony_package }}"
      state: present

  - name: Configure Chrony
    block:
    - name: Create driftfile directory if needed
      file:
        path: /var/lib/chrony
        state: directory
        mode: '0755'
        owner: chrony
        group: chrony

    - name: Configure Chrony
      blockinfile:
        path: "{{ chrony_confdir }}/chrony.conf"
        block: |
          server 0.fr.pool.ntp.org iburst
          server 1.fr.pool.ntp.org iburst
          server 2.fr.pool.ntp.org iburst
          server 3.fr.pool.ntp.org iburst
          driftfile /var/lib/chrony/drift
          makestep 1.0 3
          rtcsync
          logdir /var/log/chrony
        marker: "# {mark} ANSIBLE MANAGED BLOCK"

  - name: Enable and start Chrony
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

<br>
<br>

*Cette marmotte dodu se demande ce qu'est ansible*

*Elle retourne dans son terrier*

<img src="https://github.com/user-attachments/assets/c3f92569-1c16-4b34-bec5-758b8eadfe72" alt="bowober-removebg-preview" width="25%" height="auto">
