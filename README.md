# linux-remote-assistance

The remote assistance control system is two one-liners in the bash language and ssh server with no-shell user. Graphical shell is zenity.

## Fast start (ansible automation)

### Server configuration

1. Logon to the server, get root access.
2. apt install git ansible (Ubuntu 20.04) or dnf install git ansible (RHEL 8, Centos 8, Oracle linux 8)
3. ansible-play https://github.com/skosachiov/linux-remote-assistance/remote-assistance-deploy.yml -tag server --extra-vars "fqdn_sshserver=my.own.server no_shell_pass=deploysecret"

### Client configuration

1. Logon to the client, get root access.
2. apt install git ansible (Ubuntu 20.04) or dnf install git ansible (RHEL 8, Centos 8, Oracle linux 8)
3. ansible-play https://github.com/skosachiov/linux-remote-assistance/remote-assistance-deploy.yml -tag client --extra-vars "fqdn_sshserver=my.own.server no_shell_pass=deploysecret"

## Manual server configuration

### No-shell user preparation
* groupadd -g 1900 no-shell
* useradd -m -g 1900 -u 1900 no-shell
* passwd no-shell (for ansible automation)
* su - no-shell
* ssh-keygen
* exit
* usermod --shell /bin/false no-shell
* cp /home/no-shell/.ssh/id_rsa.pub /home/no-shell/.ssh/authorized_keys
* ssh no-shell@openssh.example.com

### We need for clients configuration
* /home/no-shell/.ssh/id_rsa
* last line in file ~/.ssh/known_hosts

## Manual client (user and helpdesk) configuration

* dnf or apt install x11vnc zenity tigervnc
* cp id_rsa /usr/local/etc/
* chown nobody /usr/loca/etc/id_rsa
* cd /usr/share/applications/
* wget this repo *.desktop files
* sed -i "s/openssh.example.com/your.server/g" remote-assistance-*
