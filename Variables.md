# Exercice

Les VM tournent toutes sous Rocky Linux. Démarrez-les :

```$ vagrant up```

Connectez-vous au Control Host :

```$ vagrant ssh control```

Rendez-vous dans le répertoire du projet :

```$ cd ansible/projets/ema/```

Écrivez un playbook myvars1.yml qui affiche respectivement votre voiture et votre moto préférée en utilisant le module debug et deux variables mycar et mybike définies en tant que play vars.

```$ nano myvars1.yml```

```yml
---
- name: Display my favorite car and bike
  hosts: localhost
  vars:
    mycar: "Tesla Model S"
    mybike: "Ducati Panigale V4"
  tasks:
    - name: Show my favorite car
      debug:
        msg: "My favorite car is {{ mycar }}"

    - name: Show my favorite bike
      debug:
        msg: "My favorite bike is {{ mybike }}"
...
```
En utilisant les extra vars, remplacez successivement l’une et l’autre marque – puis les deux à la fois – avant d’exécuter le play.

```
$ ansible-playbook myvars1.yml -e "mycar='BMW M5'"
$ ansible-playbook myvars1.yml -e "mybike='Kawasaki Ninja ZX-10R'"
$ ansible-playbook myvars1.yml -e "mycar='Porsche 911' mybike='Kawasaki Ninja ZX-10R'"
```
    
Écrivez un playbook myvars2.yml qui fait essentiellement la même chose que myvars1.yml, mais en utilisant une tâche avec set_fact pour définir les deux variables.

```$ nano myvars2.yml```
```yml
---
- name: Display my favorite car and bike using set_fact
  hosts: localhost
  tasks:
    - name: Set mycar and mybike variables
      set_fact:
        mycar: "Tesla Model S"
        mybike: "Ducati Panigale V4"

    - name: Show my favorite car
      debug:
        msg: "My favorite car is {{ mycar }}"

    - name: Show my favorite bike
      debug:
        msg: "My favorite bike is {{ mybike }}"
...
```
Là aussi, essayez de remplacer les deux variables en utilisant des extra vars avant l’exécution du play.

```$ ansible-playbook myvars2.yml -e "mycar='BMW M5' mybike='Yamaha R1'"```

    
Écrivez un playbook myvars3.yml qui affiche le contenu des deux variables mycar et mybike mais sans les définir. Avant d’exécuter le playbook, définissez VW et BMW comme valeurs par défaut pour mycar et mybike pour tous les hôtes, en utilisant l’endroit approprié.

```$ nano myvars3.yml```
```yml
---
- name: Display mycar and mybike variables
  hosts: localhost
  tasks:
    - name: Show my favorite car
      debug:
        msg: "My favorite car is {{ mycar }}"

    - name: Show my favorite bike
      debug:
        msg: "My favorite bike is {{ mybike }}"
...
```

```
$ mkdir -v ~/ansible/projets/ema/group_vars
$ cd group_vars/
$ nano all.yml
```
```yml
---  # group_vars/all.yml

mycar: VW   
mybike: BMW

...
```
```
$ cd ../
$ ansible-playbook myvars3.yml
```
    
Effectuez le nécessaire pour remplacer VW et BMW par Mercedes et Honda sur l’hôte target02.

```
$ mkdir -v ~/ansible/projets/ema/host_vars
$ cd host_vars/
$ nano target02.yml
```
```yml
---  # group_vars/target02.yml

mycar: Mercedes
mybike: Honda

...
```
```
$ cd ../
$ ansible-playbook myvars3.yml
```
```yml
---
- name: Display mycar and mybike variables
  hosts: all
  tasks:
    - name: Show my favorite car
      debug:
        msg: "My favorite car is {{ mycar }}"

    - name: Show my favorite bike
      debug:
        msg: "My favorite bike is {{ mybike }}"
...
```
    
Écrivez un playbook display_user.yml qui affiche un utilisateur et son mot de passe correspondant à l’aide des variables user et password. Ces deux variables devront être saisies de manière interactive pendant l’exécution du playbook. Les valeurs par défaut seront microlinux pour user et yatahongaga pour password. Le mot de passe ne devra pas s’afficher pendant la saisie.

```
$ cd playbooks/
$ nano display_user.yml
```
```yml
---
- name: Display user and password interactively
  hosts: localhost
  gather_facts: false
  vars_prompt:
    - name: user
      prompt: "Enter the username"
      default: "microlinux"
      private: false  # Pas de masquage pour l'utilisateur
    - name: password
      prompt: "Enter the password"
      default: "yatahongaga"
      private: true  # Masquage pour le mot de passe
  tasks:
    - name: Display the entered username
      debug:
        msg: "The entered username is {{ user }}"

    - name: Display the entered password
      debug:
        msg: "The entered password is {{ password }}"

...
```
