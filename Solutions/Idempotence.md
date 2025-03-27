# Exercice

Installez successivement les paquets tree, git et nmap sur toutes les cibles.

```$ ansible all -m package -a "name=tree,git,nmap"```

Désinstallez successivement ces trois paquets en utilisant le paramètre supplémentaire state=absent.

```$ ansible all -m package -a "name=tree,git,nmap state=absent"```

Copier le fichier /etc/fstab du Control Host vers tous les Target Hosts sous forme d’un fichier /tmp/test3.txt.

```$ ansible all -m copy -a "src=/etc/fstab dest=/tmp/test3.txt"```

Supprimez le fichier /tmp/test3.txt sur les Target Hosts en utilisant le module file avec le paramètre state=absent.

```$ ansible all -m file -a "path=/tmp/test3.txt state=absent"```

Enfin, affichez l’espace utilisé par la partition principale sur tous les Target Hosts. Que remarquez-vous ?

```$ ansible all -m command -a "df -h /"```

On remarque que la couleur est orange comme si il y avait eu un changement dans la configuration alors que ce n'est pas le cas.
