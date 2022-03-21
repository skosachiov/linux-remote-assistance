# Linux remote assistance

The ansible code produces desktop files in the /usr/share/applications/ folder and creates server no-shell user. Desktop files contain one-liners bash scripts. Desktop files allow in automatic mode to connect reverse forward and direct forward ports over ssh for telework and helpdesk, telework launches x11vnc with a pin code, helpdesk "sees" telework vnc on helpdesk local port. Forward ports through ssh server allows you to provide assistance behind NAT. Graphical shell is zenity. Dependences are x11vnc, tigervnc, sshpasswd, zenity.

![Linux remote assistance](https://github.com/skosachiov/linux-remote-assistance/raw/main/remote-assistance-scheme.png)

## Fast start (ansible automation)

### Server configuration
1. Logon to the server and get root access
2. apt install git ansible (Ubuntu 20.04) or dnf install git ansible (RHEL 8, Centos 8, Oracle linux 8)
3. echo "localhost ansible_connection=local" >> /etc/ansible/hosts
4. ansible-pull --tag server --extra-vars "fqdn_sshserver=my.ssh.example.com no_shell_pass=deploysecret" -U https://github.com/skosachiov/linux-remote-assistance

### Client configuration
1. Logon to the client and get root access
2. apt install git ansible (Ubuntu 20.04) or dnf install git ansible (RHEL 8, Centos 8, Oracle linux 8)
3. echo "localhost ansible_connection=local" >> /etc/ansible/hosts
4. ansible-pull --tag client --extra-vars "fqdn_sshserver=my.ssh.example.com no_shell_pass=deploysecret" -U https://github.com/skosachiov/linux-remote-assistance


