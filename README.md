# Linux remote assistance
The remote assistance control system is two one-liners in the bash language and ssh server with no-shell user. Graphical shell is zenity.

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


