# Exercice



Écrivez trois playbooks pour afficher des informations sur chacun des Target Hosts :

pkg-info.yml pour afficher le gestionnaire de paquets utilisé

```$ nano pkg-info.yml```

```yml
---
- name: Afficher le gestionnaire de paquets utilisé
  hosts: all
  gather_facts: true
  tasks:
    - name: Afficher le gestionnaire de paquets
      debug:
        msg: "Le gestionnaire de paquets utilisé est : {{ ansible_pkg_mgr }}"
...
```
```$ ansible-playbook pkg-info.yml```

python-info.yml pour afficher la version de Python installée

```$ nano python-info.yml```
```yml
---
- name: Afficher la version de Python installée
  hosts: all
  gather_facts: true
  tasks:
    - name: Afficher la version de Python
      debug:
        msg: "La version de Python installée est : {{ ansible_python_version }}"
...

```
```$ ansible-playbook python-info.yml ```

dns-info.yml pour afficher le(s) serveur(s) DNS utilisé(s)

```$ nano dns-info.yml```
```yml
---
- name: Display DNS information
  hosts: all
  gather_facts: yes
  tasks:
    - name: Display DNS servers
      debug:
        msg: "The DNS servers are: {{ ansible_facts.dns.nameservers }}"

...
```
```$ ansible-playbook dns-info.yml ```
