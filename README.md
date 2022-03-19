# linux-remote-assistance

The control system is two one-liners in the bash language and ssh server with no-shell user. Graphical shell is zenity.

## Server configuration

(Ansible automation coming soon)

### No-shell user preparation
* groupadd -g 1900 no-shell
* useradd -m -g 1900 -u 1900 no-shell
* passwd no-shell (for ansible automation)
* su - no-shell
* ssh-keygen
* exit
* cp /home/no-shell/.ssh/id_rsa.pub /home/no-shell/.ssh/authorized_keys
* ssh no-shell@openssh.example.com

### We need for clients configuration
* /home/no-shell/.ssh/id_rsa
* last line in file ~/.ssh/known_hosts

## Client (user and helpdesk) configuration

(Ansible automation coming soon)

* dnf or apt install x11vnc zenity tigervnc
* cp id_rsa /usr/local/etc/
* chown nobody /usr/loca/etc/id_rsa
* cd /usr/share/applications/
* wget this repo *.desktop files
* sed -i "s/openssh.example.com/your.server/g" remote-assistance-*
