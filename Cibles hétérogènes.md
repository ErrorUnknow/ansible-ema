# Exercice

```$ nano chrony-01.yml```

```yaml
---
- name: Install and configure Chrony on different distributions
  hosts: all
  become: true

  vars:
    chrony_confdir: "/etc/chrony.conf"

  tasks:

    - name: Update package information on Debian/Ubuntu
      apt:
        update_cache: true
        cache_valid_time: 3600
      when: ansible_os_family == "Debian"

    - name: Install chrony on Debian/Ubuntu
      apt:
        name: chrony
        state: present
      when: ansible_os_family == "Debian"

    - name: Install chrony on Rocky Linux
      dnf:
        name: chrony
        state: present
      when: ansible_distribution == "Rocky"

    - name: Install chrony on SUSE Linux
      zypper:
        name: chrony
        state: present
      when: ansible_distribution == "openSUSE Leap"

    - name: Set chrony.conf path for Rocky Linux
      set_fact:
        chrony_confdir: "/etc/chrony.conf"
      when: ansible_distribution == "Rocky"

    - name: Set chrony.conf path for SUSE Linux
      set_fact:
        chrony_confdir: "/etc/chrony.conf"
      when: ansible_distribution == "openSUSE Leap"

    - name: Set chrony.conf path for Debian/Ubuntu
      set_fact:
        chrony_confdir: "/etc/chrony/chrony.conf"
      when: ansible_os_family == "Debian"

    - name: Ensure /etc/chrony directory exists on Debian/Ubuntu
      file:
        path: /etc/chrony
        state: directory
        mode: '0755'
      when: ansible_os_family == "Debian"

    - name: Copy chrony.conf to the correct location
      copy:
        dest: "{{ chrony_confdir }}"
        content: |
          server 0.fr.pool.ntp.org iburst
          server 1.fr.pool.ntp.org iburst
          server 2.fr.pool.ntp.org iburst
          server 3.fr.pool.ntp.org iburst
          driftfile /var/lib/chrony/drift
          makestep 1.0 3
          rtcsync
          logdir /var/log/chrony
      notify: restart chrony service

    - name: Ensure chrony service is enabled and started
      systemd:
        name: chronyd
        enabled: true
        state: started

  handlers:
    - name: restart chrony service
      systemd:
        name: chronyd
        state: restarted

```
```$ ansible-playbook chrony-01.yml```
