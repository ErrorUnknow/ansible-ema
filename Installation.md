# Exercice 1

Démarrez la VM ubuntu depuis le répertoire atelier-01.

```$ vagrant up debian```

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

```ansible 2.10.8
  config file = None
  configured module search path = ['/home/vagrant/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules']
  ansible python module location = /usr/lib/python3/dist-packages/ansible
  executable location = /usr/bin/ansible
  python version = 3.10.12 (main, Jan 17 2025, 14:35:34) [GCC 11.4.0]
```

Déconnectez-vous et supprimez la VM.

```$ exit
$ vagrant destroy -f ubuntu```

