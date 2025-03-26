# Fasts

## Exercice

Voici le playbook ```pkg-info.yml``` :

```yaml
---
- name: Display package manager information
  hosts: testing
  gather_facts: yes

  tasks:
    - name: Show package manager
      debug:
        msg: "Host {{ inventory_hostname }} uses {{ ansible_pkg_mgr }} as package manager."
```

Resultat : 

```shell
TASK [Show package manager] ****************************************************
ok: [rocky] => {
    "msg": "Host rocky uses dnf as package manager."
}
ok: [debian] => {
    "msg": "Host debian uses apt as package manager."
}
ok: [suse] => {
    "msg": "Host suse uses zypper as package manager."
}
```

---

Voici le playbook ```python-info.yml``` :

```yaml
---
- name: Display Python version
  hosts: all
  gather_facts: yes

  tasks:
    - name: Get Python version
      block:
        - name: Try python command
          shell: python --version
          register: python_version
          ignore_errors: yes

        - name: Try python3 command if python fails
          shell: python3 --version
          register: python3_version
          when: python_version is failed

        - name: Display Python version
          debug:
            msg: "Host {{ inventory_hostname }} has Python version {{ python_version.stdout if python_version is succeeded else python3_version.stdout }}"
```

Resultat :

```shell
TASK [Display Python version] ***********************************************************************************
ok: [rocky] => {
    "msg": "Host rocky has Python version Python 3.9.18"
}
ok: [debian] => {
    "msg": "Host debian has Python version Python 3.11.2"
}
ok: [suse] => {
    "msg": "Host suse has Python version Python 3.6.15"
}
```

---

Voici le playbook ```dns-info.yml``` :

```yaml
---
- name: Display DNS servers
  hosts: testing
  gather_facts: yes

  tasks:
    - name: Get DNS servers
      shell: grep nameserver /etc/resolv.conf
      register: dns_servers

    - name: Display DNS servers
      debug:
        msg: "Host {{ inventory_hostname }} uses DNS servers: {{ dns_servers.stdout }}"
```

Resultat : 

```shell
TASK [Display DNS servers] **************************************************************************************
ok: [rocky] => {
    "msg": "Host rocky uses DNS servers: nameserver 10.0.2.3"
}
ok: [debian] => {
    "msg": "Host debian uses DNS servers: # run \"resolvectl status\" to see details about the actual nameservers.\nnameserver 4.2.2.1\nnameserver 4.2.2.2\nnameserver 208.67.220.220"
}
ok: [suse] => {
    "msg": "Host suse uses DNS servers: nameserver 10.0.2.3"
}
```
