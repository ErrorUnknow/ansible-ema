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
  hosts: redhat  # Cible le groupe 'redhat' dans l'inventaire
  become: yes
  tasks:
    - name: Install chrony package
      package:
        name: chrony
        state: present

    - name: Enable and start the chronyd service
      service:
        name: chronyd
        state: started
        enabled: yes

    - name: Backup the default /etc/chrony.conf
      copy:
        src: /etc/chrony.conf
        dest: /etc/chrony.conf.bak
        remote_src: yes
        backup: yes

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

    - name: Restart chronyd service to apply new configuration
      service:
        name: chronyd
        state: restarted

    - name: Check chrony service status
      command: systemctl status chronyd
      register: chrony_status
      failed_when: "'Active: active (running)' not in chrony_status.stdout"

    - name: Output chrony service status
      debug:
        var: chrony_status.stdout
...
```
