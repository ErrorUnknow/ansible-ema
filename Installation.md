# Exercice 1

Démarrez la VM ubuntu depuis le répertoire atelier-01.

```$ vagrant up ubuntu```

Connectez-vous à cette VM.

```$ vagrant ssh ubuntu```

Rafraîchissez les informations sur les paquets.

```$ sudo apt update```

Recherchez le paquet ansible avec les options qui vont bien.

```$ apt-cache search --names-only ansible```

Installez le paquet officiel fourni par la distribution.

```$ sudo apt install -y ansible```

Vérifiez si l’installation s’est bien déroulée.

```$ ansible --version```

Notez la version d’Ansible.

```
vagrant@ubuntu:~$ ansible --version
 ansible 2.10.8
 config file = None
 configured module search path = ['/home/vagrant/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules']
 ansible python module location = /usr/lib/python3/dist-packages/ansible
 executable location = /usr/bin/ansible
 python version = 3.10.12 (main, Jan 17 2025, 14:35:34) [GCC 11.4.0]
```

Déconnectez-vous et supprimez la VM.

```
$ exit
$ vagrant destroy -f ubuntu
```
# Exercice 2

Démarrez la VM ubuntu depuis le répertoire atelier-01.

```$ vagrant up ubuntu```

Connectez-vous à cette VM.

```$ vagrant ssh ubuntu```

Rafraîchissez les informations sur les paquets.

```$ sudo apt update```

Répétez l’exercice précédent en configurant un dépôt PPA (Personal Package Archive) pour Ansible :

```
$ sudo apt-add-repository ppa:ansible/ansible
$ sudo apt update
```

Recherchez le paquet ansible avec les options qui vont bien.

```$ apt-cache search --names-only ansible```

Installez le paquet officiel fourni par la distribution.

```$ sudo apt install -y ansible```

Vérifiez si l’installation s’est bien déroulée.

```$ ansible --version```

Notez la version d’Ansible.

```
vagrant@ubuntu:~$ ansible --version
ansible [core 2.17.9]
  config file = /etc/ansible/ansible.cfg
  configured module search path = ['/home/vagrant/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules']
  ansible python module location = /usr/lib/python3/dist-packages/ansible
  ansible collection location = /home/vagrant/.ansible/collections:/usr/share/ansible/collections
  executable location = /usr/bin/ansible
  python version = 3.10.12 (main, Jan 17 2025, 14:35:34) [GCC 11.4.0] (/usr/bin/python3)
  jinja version = 3.0.3
  libyaml = True

```

Déconnectez-vous et supprimez la VM.

```
$ exit
$ vagrant destroy -f ubuntu
```
# Exercice 3

Lancez une VM Rocky Linux et installez Ansible en utilisant PIP et Virtualenv.

```
$ vagrant up rocky
$ vagrant ssh rocky
$ sudo dnf install -y python3-pip
$ python3 -m venv ~/.venv/ansible
$ source ~/.venv/ansible/bin/activate
(ansible) $ pip install --upgrade pip
(ansible) $ pip install ansible
(ansible) $ ansible --version
(ansible) $ deactivate
$ exit
$ vagrant destroy -f rocky
```


