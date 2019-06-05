# Ansible_tutorial
This is the full steps that you can do to implement ansible in debian 9 stretch

Steps:

1.	Create a Debian stretch VM with the hostname control-server (1st VM)
2.	Create a Debian stretch VM with the hostname client-server (2nd VM)
3.	In the 1st VM do: “apt install ansible”
4.	In 1st VM do : “useradd -d /home/ansadm -m ansadm”
5.	Then in 1st VM set password of user ansadm by : “passwd ansadm”
6.	In client-server ( 2nd VM) do step 4-5.
7.	In control-server (1st vm) do (to login as user ansadm) : “su – ansadm” 
8.	Then in 1st vm itself do : “ssh-keygen -t rsa” press enter till $ sign appear
9.	Do : “cat /home/ansadm/.ssh/id_rsa.pub” , then copy the key
10.	Do step 7 in 2nd vm as well
11.	In 2nd vm create directory .ssh using : “mkdir .ssh”
12.	In 2nd vm do: “chmod 700 .ssh/” then “chown ansadm:ansadm .ssh/”
13.	In 2nd vm do: “cd .ssh/” then “nano authorized_keys” and paste the key from step 9 in it and save it.
14.	Change ownership of authorized_keys by: “chown ansadm:ansadm authorized_keys” and “chmod 600 authorized_keys”
15.	Test whether keys is working, in 1st vm do : “ssh ip(of 2nd vm)”, it should connect without password, then do “exit”
16.	in 1st vm, do: “exit” then “chown -R ansadm:ansadm /etc/ansible”
17.	do step 7 in 1st vm
18.	do “vim /etc/ansible/hosts” and add this at the bottom :

[webservers]
Ip of 2nd vm

19.	in 1st vm do “ansible webservers -m ping” to test if 2nd vm connects
20.	in 2nd vm give ansadm sudo root
20.1.	do “apt install sudo”
20.2.	“nano /etc/sudoers”
20.3.	Add “ansadm ALL=NOPASSWD: ALL” at the bottom


For playbook(all should be done in 1st vm)
21.	In 1st vm do step 7 then “cd /etc/ansible”
22.	Create file by using : “cat > filename.yml” *note: replace filename by any name u want*
23.	Do “nano filename.yml” *note: filename should be the same name which is in step 22*



24.	Paste this in the file:

---
- hosts: webservers
  user: ansadm
  become: yes
  become_method: sudo
  tasks:
         - name: Install MariaDB
           apt: name=mariadb-server state=latest
           
         - name: Install Nginx
           apt: name=nginx state=latest

         - name: Install PHP-fpm
           apt: name=php-fpm state=latest

         - name: Install Redmine
           apt: name=redmine-mysql state=latest

25.	Save file with extension .yml
26.	Do “ansible-playbook filename.yml”
