# Exercice

Placez-vous dans le répertoire du dixième atelier pratique :

```$ cd ~/formation-ansible/atelier-10```

Démarrez les VM :

```$ vagrant up```

Connectez-vous au Control Host :

```$ vagrant ssh ansible```

```
$ cd ansible/projets/ema/
```
Un premier playbook apache-debian.yml qui installe Apache sur l’hôte debian avec une page personnalisée Apache web server running on Debian Linux.
```
$ nano apache-debian.yml
$ yamllint apache-debian.yml
$ ansible-playbook apache-debian.yml
```

```
---  # apache-debian.yml
- name: Installer Apache et configurer une page personnalisée sur Debian
  hosts: debian
  become: true
  tasks:
    - name: Installer Apache sur Debian
      ansible.builtin.apt:
        name: apache2
        state: present
        update_cache: true

    - name: Créer une page HTML personnalisée pour Apache
      ansible.builtin.copy:
        dest: /var/www/html/index.html
        content: "Apache web server running on Debian Linux"

    - name: Assurer que le service Apache est démarré et activé
      ansible.builtin.systemd:
        name: apache2
        state: started
        enabled: true
...
```
Un deuxième playbook apache-rocky.yml qui installe Apache sur l’hôte rocky avec une page personnalisée Apache web server running on Rocky Linux.
```
$ nano apache-debian.yml
$ yamllint apache-rocky.yml
$ ansible-playbook apache-rocky.yml
```

```
---  # apache-rocky.yml
- name: Installer Apache et configurer une page personnalisée sur Rocky Linux
  hosts: rocky
  become: true
  tasks:
    - name: Installer Apache sur Rocky Linux
      ansible.builtin.dnf:
        name: httpd
        state: present
        update_cache: true

    - name: Créer une page HTML personnalisée pour Apache
      ansible.builtin.copy:
        dest: /var/www/html/index.html
        content: "Apache web server running on Rocky Linux"

    - name: Assurer que le service Apache est démarré et activé
      ansible.builtin.systemd:
        name: httpd
        state: started
        enabled: true
...
```
Un troisième playbook apache-suse.yml qui installe Apache sur l’hôte suse avec une page personnalisée Apache web server running on SUSE Linux.
```
$ nano apache-debian.yml
$ yamllint apache-suse.yml
$ ansible-playbook aapache-suse.yml
```
```
---  # apache-suse.yml
- name: Installer Apache et configurer une page personnalisée sur SUSE Linux
  hosts: suse
  become: true
  tasks:
    - name: Installer Apache sur SUSE Linux
      ansible.builtin.zypper:
        name: apache2
        state: present
        update_cache: true

    - name: Créer une page HTML personnalisée pour Apache
      ansible.builtin.copy:
        dest: /srv/www/htdocs/index.html
        content: "Apache web server running on SUSE Linux"

    - name: Assurer que le service Apache est démarré et activé
      ansible.builtin.systemd:
        name: apache2
        state: started
        enabled: true
...
```
