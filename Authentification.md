# Exercice

Démarrez les VM :

```$ vagrant up```

Connectez-vous au Control Host :

````$ vagrant ssh control```

Ansible est déjà installé sur cette machine :

```
$ type ansible
ansible is /usr/bin/ansible
```
Faites le nécessaire pour réussir un ping Ansible comme ceci :

```
$ ansible all -i target01,target02,target03 -m ping
target03 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
target02 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
target01 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
```
