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

    - name: Display the kernel information
      debug:
        msg: "Kernel Information: {{ kernel_info.stdout }}"                                     
...
```
