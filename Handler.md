# Exercice

Les VM tournent toutes sous Rocky Linux. Démarrez-les :

```$ vagrant up```

Connectez-vous au Control Host :

```$ vagrant ssh control```

Rendez-vous dans le répertoire du projet :

```$ cd ansible/projets/ema/```

Écrivez un playbook chrony.yml qui assure la synchronisation NTP de tous vos Target Hosts :

```yml
---
- name: Configure Chrony NTP service
  hosts: redhat
  become: true
  tasks:
    - name: Install chrony package
      package:
        name: chrony
        state: present
      notify: Restart chronyd

    - name: Enable and start the chronyd service
      systemd:
        name: chronyd
        state: started
        enabled: true
      notify: Restart chronyd

    - name: Backup the default /etc/chrony.conf
      copy:
        src: /etc/chrony.conf
        dest: /etc/chrony.conf.bak
        remote_src: true
        backup: true

    - name: Install custom chrony configuration
      copy:
        dest: /etc/chrony.conf
        content: |
          # /etc/chrony.conf
          server 0.fr.pool.ntp.org iburst
          server 1.fr.pool.ntp.org iburst
          server 2.fr.pool.ntp.org iburst
          server 3.fr.pool.ntp.org iburst
          driftfile /var/lib/chrony/drift
          makestep 1.0 3
          rtcsync
          logdir /var/log/chrony
        owner: root
        group: root
        mode: '0644'
      notify: Restart chronyd

    - name: Check chrony service status
      command: systemctl status chronyd
      register: chrony_status
      failed_when: "'Active: active (running)' not in chrony_status.stdout"

    - name: Output chrony service status
      debug:
        var: chrony_status.stdout

  handlers:
    - name: Restart chronyd
      systemd:
        name: chronyd
        state: restarted
...
```
Vérifiez la syntaxe correcte de votre playbook chrony.yml.

```$ yamllint chrony.yml```

Vérifiez l’idempotence de votre playbook.
```
$ ansible-playbook chrony.yml
$ ansible-playbook chrony.yml
```


