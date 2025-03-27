# Exercice

Écrivez un playbook chrony.yml qui installe un fichier de configuration personnalisé sur vos cibles. La première ligne de commentaire devra indiquer le chemin complet vers le fichier :

Dans certains cas ce sera /etc/chrony/chrony.conf.
Dans d’autres cas ce sera simplement /etc/chrony.conf.


```
$ cd templates/
$ nano chrony.conf.j2
```

```
# chrony.conf
server 0.fr.pool.ntp.org iburst
server 1.fr.pool.ntp.org iburst
server 2.fr.pool.ntp.org iburst
server 3.fr.pool.ntp.org iburst
driftfile /var/lib/chrony/drift
makestep 1.0 3
rtcsync
logdir /var/log/chrony
```

```$ cd ../```

```$ nano chrony.yml```

```yaml
---
- name: Install and configure Chrony with custom configuration
  hosts: all
  become: true

  vars:
    chrony_conf_path_debian_ubuntu: "/etc/chrony.conf"
    chrony_conf_path_suse: "/etc/chrony.conf"
    chrony_conf_path_rocky: "/etc/chrony.conf"
    chrony_service_debian_ubuntu: "chrony"
    chrony_service_suse: "chronyd"
    chrony_service_rocky: "chronyd"

  tasks:
    # Install chrony package if it's not already installed
    - name: Install chrony package
      package:
        name: chrony
        state: present

    # Set chrony.conf path for Debian/Ubuntu
    - name: Set chrony.conf path for Debian/Ubuntu
      set_fact:
        chrony_conf_path: "{{ chrony_conf_path_debian_ubuntu }}"
        chrony_service: "{{ chrony_service_debian_ubuntu }}"
      when: ansible_os_family == "Debian"

    # Set chrony.conf path for SUSE Linux
    - name: Set chrony.conf path for SUSE Linux
      set_fact:
        chrony_conf_path: "{{ chrony_conf_path_suse }}"
        chrony_service: "{{ chrony_service_suse }}"
      when: ansible_distribution == "openSUSE Leap"

    # Set chrony.conf path for Rocky Linux
    - name: Set chrony.conf path for Rocky Linux
      set_fact:
        chrony_conf_path: "{{ chrony_conf_path_rocky }}"
        chrony_service: "{{ chrony_service_rocky }}"
      when: ansible_distribution == "Rocky"

    # Copy custom chrony.conf to the correct location
    - name: Copy custom chrony.conf to the correct location
      template:
        src: "templates/chrony.conf.j2"
        dest: "{{ chrony_conf_path }}"
        mode: "0644"

    # Ensure chrony service is enabled and started
    - name: Ensure chrony service is enabled and started
      systemd:
        name: "{{ chrony_service }}"
        enabled: true
        state: started

  handlers:
    - name: restart chrony service
      systemd:
        name: "{{ chrony_service }}"
        state: restarted
...
```

```$ yamllint chrony.yml```

```$ ansible-playbook chrony.yml```
