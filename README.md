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
