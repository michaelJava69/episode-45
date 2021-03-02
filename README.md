## Episode #45 - Learning Ansible with Vagrant (Part 2/4)

Code for [Episode #45 - Learning Ansible with Vagrant (Part 2/4)](https://sysadmincasts.com/episodes/45-learning-ansible-with-vagrant-part-2-4).

```
ansible web1 -m ping 

ansible --version
ansible will look for inventory.ini file
                      anasible.cfg       first it looks in 
                                                                  the present dir
                                                                  users home directory
                                                                  /etc/ansible/   contanins the master ansible.cfg
vagrant@mgmt:~$ cat ansible.cfg
[defaults]
hostfile = /home/vagrant/inventory.ini     

turns out we use -i hosts to point at inventory in same folder
cp inventory.ini hosts

ansible -i hosts all -m ping     : works 
ansible  all -m ping     : does not works 

ansible -i hosts all -m ping  --ask-pass
SSH password: vagrant
[WARNING]: Found both group and host with same name: lb
web2 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
    },
    "changed": false,
    "ping": "pong"
}
lb | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
    },
    "changed": false,
    "ping": "pong"
}
web1 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
    },
    "changed": false,
    "ping": "pong"
}

##
#  show you why you are able to connect without adding password for a bit
#################

When not cahced and have to put password in again
vagrant@mgmt:~$ sudo netstat -nap | grep EST
tcp        0      0 10.0.2.15:22            10.0.2.2:53352          ESTABLISHED 2883/sshd: vagrant
vagrant@mgmt:~$ sudo netstat -ap | grep EST
tcp        0      0 mgmt:ssh                10.0.2.2:53352          ESTABLISHED 2883/sshd: vagrant


When cahced
vagrant@mgmt:~$ sudo netstat -ap | grep EST
tcp        0      0 mgmt:36002              lb:ssh                  ESTABLISHED 3710/e51df67eb4 [mu
tcp        0      0 mgmt:ssh                10.0.2.2:53352          ESTABLISHED 2883/sshd: vagrant
tcp        0      0 mgmt:34752              web2:ssh                ESTABLISHED 3713/3eee52bc5e [mu
tcp        0      0 mgmt:35707              web1:ssh                ESTABLISHED 3708/ae750fdd40 [mu
vagrant@mgmt:~$ sudo netstat -nap | grep EST
tcp        0      0 10.0.15.10:36002        10.0.15.11:22           ESTABLISHED 3710/e51df67eb4 [mu
tcp        0      0 10.0.2.15:22            10.0.2.2:53352          ESTABLISHED 2883/sshd: vagrant
tcp        0      0 10.0.15.10:34752        10.0.15.22:22           ESTABLISHED 3713/3eee52bc5e [mu
tcp        0      0 10.0.15.10:35707        10.0.15.21:22           ESTABLISHED 3708/ae750fdd40 [mu

  PID TTY      STAT   TIME COMMAND
 2955 ?        S      0:01 sshd: vagrant@pts/0
 2956 pts/0    Ss     0:00 -bash
 3798 ?        Ss     0:00 ssh: /home/vagrant/.ansible/cp/3eee52bc5e [mux]
 3801 ?        Ss     0:00 ssh: /home/vagrant/.ansible/cp/ae750fdd40 [mux]
 3804 ?        Ss     0:00 ssh: /home/vagrant/.ansible/cp/e51df67eb4 [mux]
 3854 pts/0    R+     0:00 ps x


Prevent having to put password in
vagrant@mgmt:~$ cat e45-ssh-addkey.yml
---
- hosts: all
  become_user: root
  gather_facts: no
  remote_user: vagrant

  tasks:

  - name: install ssh key
    authorized_key: user=vagrant
                    key="{{ lookup('file', '/home/vagrant/.ssh/id_rsa.pub') }}"

Explanation
-----------

gather_facts : energy consuming : gathers facts off remote machine like histname etc
want to sudo to carry out some tasks
want to be user vagrant
all hosts

authorised_key module being used
this module is named
requires a key that we will say in in .ssh folder

Action: copies the public key of mgmt server over into the autherised_keys on each of the hosts
======
Now can ssh without password  :  
ansible -i hosts all -m ping

Pre-step create id_rsa.pub
--------------------------
ssh-keygen -t rsa -b 2048
ssh-keygen -t rsa 
ssh-keygen

 ansible-playbook -i hosts e45-ssh-addkey.yml --ask-pass
 --------------------------------------------------------

PLAY [all] *********************************************************************************************************************************************************************************

TASK [install ssh key] *********************************************************************************************************************************************************************************
changed: [web2]
changed: [lb]
changed: [web1]

PLAY RECAP *********************************************************************************************************************************************************************************
lb                         : ok=1    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
web1                       : ok=1    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
web2                       : ok=1    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

 




On mgmt  box
# vagrant environment nodes
cat /etc/hosts

10.0.15.10  mgmt
10.0.15.11  lb
10.0.15.21  web1
10.0.15.22  web2
10.0.15.23  web3
10.0.15.24  web4
10.0.15.25  web5
10.0.15.26  web6
10.0.15.27  web7
10.0.15.28  web8
10.0.15.29  web9

Some commands
=============
-m  ansible modules  .... 

```
