# Exercice

Écrivez un playbook kernel.yml qui affiche les infos détaillées du noyau sur tous vos Target Hosts. Utilisez la commande uname -a et le module debug avec le paramètre msg.

```$ nano kernel.yml```
```
---
- name: Display kernel information on all target hosts
  hosts: all                                          
  gather_facts: false                                                              
  tasks:
    - name: Get kernel information using uname -a
      command: uname -a
      register: kernel_info
      changed_when: false                                                        

    - name: Display the kernel information
      debug:
        msg: "Kernel Information: {{ kernel_info.stdout }}"                                     
...
```
```$ ansible-playbook kernel.yml```

Essayez d’obtenir le même résultat en utilisant le paramètre var du module debug.

```$ nano kernel.yml```

```
---
- name: Display kernel information on all target hosts
  hosts: all
  gather_facts: false
  tasks:
    - name: Get kernel information using uname -a
      command: uname -a
      register: kernel_info
      changed_when: false

    - name: Display the kernel information
      debug:
        var: kernel_info.stdout
...
```
```$ ansible-playbook kernel.yml```

Écrivez un playbook packages.yml qui affiche le nombre total de paquets RPM installés sur les hôtes rocky et suse (rpm -qa | wc -l).

```$ nano packages.yml```

```
---
- name: Display the total number of installed RPM packages
  hosts: rocky,suse
  gather_facts: false
  tasks:
    - name: Get the total number of installed RPM packages
      shell: rpm -qa | wc -l
      register: rpm_package_count
      changed_when: false

    - name: Display the total number of RPM packages
      debug:
        msg: "Total RPM packages installed: {{ rpm_package_count.stdout }}"
...
```
```$ ansible-playbook packages.yml```

