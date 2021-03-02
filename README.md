## Episode #45 - Learning Ansible with Vagrant (Part 2/4)

Code for [Episode #45 - Learning Ansible with Vagrant (Part 2/4)](https://sysadmincasts.com/episodes/45-learning-ansible-with-vagrant-part-2-4).

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

Some commands
=============
-m  ansible modules  .... 
