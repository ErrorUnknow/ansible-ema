# Exercice

Démarrez les VM :

```$ vagrant up```

Connectez-vous au Control Host :

```$ vagrant ssh control```


Éditez /etc/hosts de manière à ce que les Target Hosts soient joignables par leur nom d’hôte simple.

```sudo nano /etc/hosts```

```
# /etc/hosts
127.0.0.1      localhost.localdomain  localhost
192.168.56.10  control.sandbox.lan    control
192.168.56.20  target01.sandbox.lan   target01
192.168.56.30  target02.sandbox.lan   target02
192.168.56.40  target03.sandbox.lan   target03
```

Configurez l’authentification par clé SSH avec les trois Target Hosts.

```
$ ssh-keyscan -t rsa target01 target02 target03 >> .ssh/known_hosts
$ ssh-keygen
$ ssh-copy-id vagrant@target01
$ ssh-copy-id vagrant@target02
$ ssh-copy-id vagrant@target03
```

Installez Ansible.

```
$ sudo apt update
$ sudo apt install -y ansible
$ ansible --version
```
```
vagrant@control:~$ ansible --version
ansible 2.10.8
  config file = None
  configured module search path = ['/home/vagrant/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules']
  ansible python module location = /usr/lib/python3/dist-packages/ansible
  executable location = /usr/bin/ansible
  python version = 3.10.12 (main, Jan 17 2025, 14:35:34) [GCC 11.4.0]
```

Envoyez un premier ping Ansible sans configuration.

```$ ansible all -i target01,target02,target03 -u vagrant -m ping```
```
vagrant@control:~$ ansible all -i target01,target02,target03 -u vagrant -m ping
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
target03 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
```

Créez un répertoire de projet ~/monprojet.

```$ mkdir ~/monprojet```

Créez un fichier vide ansible.cfg dans ce répertoire.

```
$ cd monprojet/
$ touch ansible.cfg
```

Vérifiez si ce fichier est bien pris en compte par Ansible.

```$ ansible --version | head -n 2```
```
vagrant@control:~/monprojet$ ansible --version | head -n 2
ansible 2.10.8
  config file = /home/vagrant/monprojet/ansible.cfg
```

Spécifiez un inventaire nommé hosts.

```
$ sudo nano ansible.cfg
```
```
[defaults]
inventory = ./inventory
```

Activez la journalisation dans ~/journal/ansible.log.

```
$ mkdir -v ~/logs
$ sudo nano ansible.cfg
```
```
[defaults]
inventory = ./inventory
log_path = ~/logs/ansible.log
```
Testez la journalisation.

Créez un groupe [testlab] avec vos trois Target Hosts.
Définissez explicitement l’utilisateur vagrant pour la connexion à vos cibles.
Envoyez un ping Ansible vers le groupe de machines [all].
Définissez l’élévation des droits pour l’utilisateur vagrant sur les Target Hosts.
Affichez la première ligne du fichier /etc/shadow sur tous les Target Hosts.
Quittez le Control Host et supprimez toutes les VM de l’atelier.
