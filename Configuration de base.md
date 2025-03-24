# Exercice

Démarrez les VM :

```$ vagrant up```

Connectez-vous au Control Host :

```$ vagrant ssh control```


Éditez /etc/hosts de manière à ce que les Target Hosts soient joignables par leur nom d’hôte simple.

```sudo nano /etc/hosts```

```



Configurez l’authentification par clé SSH avec les trois Target Hosts.
Installez Ansible.
Envoyez un premier ping Ansible sans configuration.
Créez un répertoire de projet ~/monprojet.
Créez un fichier vide ansible.cfg dans ce répertoire.
Vérifiez si ce fichier est bien pris en compte par Ansible.
Spécifiez un inventaire nommé hosts.
Activez la journalisation dans ~/journal/ansible.log.
Testez la journalisation.
Créez un groupe [testlab] avec vos trois Target Hosts.
Définissez explicitement l’utilisateur vagrant pour la connexion à vos cibles.
Envoyez un ping Ansible vers le groupe de machines [all].
Définissez l’élévation des droits pour l’utilisateur vagrant sur les Target Hosts.
Affichez la première ligne du fichier /etc/shadow sur tous les Target Hosts.
Quittez le Control Host et supprimez toutes les VM de l’atelier.
