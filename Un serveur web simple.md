# Exercice

Placez-vous dans le répertoire du dixième atelier pratique :

```$ cd ~/formation-ansible/atelier-10```

Démarrez les VM :

```$ vagrant up```

Connectez-vous au Control Host :

```$ vagrant ssh ansible```

```
$ cd ansible/projets/ema/
$ nano apache-debian.yml
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
