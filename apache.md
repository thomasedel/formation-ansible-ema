# Apache Config

## Exercice

On se connecte à la VM ansible : ```vagrant ssh ansible```

On se déplace dans le dossier : ```cd ansible/projets/ema/playbooks```

## Debian

On créer le playbook suivant pour debian : 

```
---
- name: Install Apache on Debian
  hosts: debian
  become: true

  tasks:
  - name: Update package cache
    apt:
      update_cache: yes

  - name: Install Apache
    apt:
      name: apache2
      state: latest

  - name: Create custom index.html
    copy:
      dest: "/var/www/html/index.html"
      content: |
        <!doctype html>
        <html>
        <head>
          <title>Je suis une page web UwU</title>
        </head>
        <body>
          <h1>Je suis une page web OwO</h1>
        </body>
        </html>

  - name: Start and enable Apache service
    service:
      name: apache2
      enabled: yes
      state: started
```

Après l'avoir lancé avec : ```ansible-playbook apache-01.yml```, on fait un curl debian.

On constate que le site fonctionne bien.

```html
[vagrant@ansible playbooks]$ curl debian
<!doctype html>
<html>
<head>
  <title>Je suis une page web UwU</title>
</head>
<body>
  <h1>Je suis une page web OwO</h1>
</body>
</html>
```
## Rocky

Pour rocky on utilise le playbook suivant :

```yaml
---
- name: Install Apache on Rocky Linux
  hosts: rocky
  become: true

  tasks:
  - name: Install Apache
    dnf:
      name: httpd
      state: latest

  - name: Create custom index.html
    copy:
      dest: "/var/www/html/index.html"
      content: |
        <!doctype html>
        <html>
        <head>
          <title>Je suis un site sous Rocky OwO</title>
        </head>
        <body>
          <h1>Je suis un site sous Rocky OwO</h1>
        </body>
        </html>

  - name: Start and enable Apache service
    service:
      name: httpd
      enabled: yes
      state: started
```

On le lance avec ```ansible-playbook apache-02.yml```

On vérifie avec  ```curl rocky``` et on constate que le serveur apache fonctionne lui aussi :

```html
[vagrant@ansible playbooks]$ curl rocky
<!doctype html>
<html>
<head>
  <title>Je suis un site sous Rocky OwO</title>
</head>
<body>
  <h1>Je suis un site sous Rocky OwO</h1>
</body>
</html>
```

## Suse

Pour suse on créer le playbook suivant : 

```yaml
---
- name: Install Apache on SUSE Linux
  hosts: suse
  become: true

  tasks:
  - name: Install Apache
    zypper:
      name: apache2
      state: latest

  - name: Create custom index.html
    copy:
      dest: "/srv/www/htdocs/index.html"
      content: |
        <!doctype html>
        <html>
        <head>
          <title>Apache on SUSE</title>
        </head>
        <body>
          <h1>Apache web server running on SUSE Linux</h1>
        </body>
        </html>

  - name: Start and enable Apache service
    service:
      name: apache2
      enabled: yes
      state: started

  - name: Open firewall for HTTP
    firewalld:
      service: http
      permanent: yes
      state: enabled
    notify: reload firewalld

  handlers:
  - name: reload firewalld
    firewalld:
      state: reloaded
```

On le lance avec ```ansible-playbook apache-03.yml```

On vérifie avec  ```curl suse``` et on constate que le serveur apache fonctionne lui aussi :

```html
[vagrant@ansible playbooks]$ curl suse
<!doctype html>
<html>
<head>
  <title>Je suis un site sur SUSE OwO</title>
</head>
<body>
  <h1>Je suis un site sur SUSE UwU</h1>
</body>
</html>
```
