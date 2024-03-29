# Linux remote assistance

The ansible code produces desktop files in the /usr/share/applications/ folder and creates server no-shell user. Desktop files contain one-liners bash scripts. Desktop files allow to connect in automatic mode reverse forward and direct forward ports over ssh for telework and helpdesk, telework launches x11vnc with a pin code, helpdesk "sees" telework vnc on helpdesk local port. Forward ports through ssh server allows you to provide assistance behind NAT. Graphical shell is zenity. Dependences are x11vnc, tigervnc, sshpasswd, zenity. X11 display server, no Wayland support yet.

![Linux remote assistance](https://github.com/skosachiov/linux-remote-assistance/raw/main/docs/remote-assistance-scheme.png)

## Fast start (ansible automation, push mode)

Tested on Ubuntu 20.04/22.04 and Centos Stream 8. Can be used for any Debian/RHEL-based distributions.

### Install

1. Clone git repo `git clone https://github.com/skosachiov/linux-remote-assistance`
2. Ensure sshd service started on client/server
3. Use `su` become method for default CentOS/RHEL installation and `sudo` for Debian/Ubuntu \
`ansible-playbook -v -k -K -b --become-method=<method> -u user -e "fqdn_sshserver=my.ssh.example.com no_shell_pass=deploysecret" -i my.ssh.example.com, server.yml` \
`ansible-playbook -v -k -K -b --become-method=<method> -u user -e "fqdn_sshserver=my.ssh.example.com no_shell_pass=deploysecret" -i client.example.com, client.yml`

### Uninstall

1. Uninstall client \
`ansible-playbook -v -k -K -b --become-method=<method> -u user -i client.example.com, client-uninstall.yml`
2. Uninstall server \
`ansible-playbook -v -k -K -b --become-method=<method> -u user -i my.ssh.example.com, server-uninstall.yml`

## Ansible automation pull mode

Tested on Ubuntu 20.04/22.04 and Centos Stream 8. Can be used for any Debian/RHEL-based distributions.

### Server configuration
1. Logon to the server and get root access
2. Install packages \
   Ubuntu: `apt install git ansible` \
   RHEL 8, Centos 8, Oracle linux 8: `dnf install epel-release; dnf install git ansible-core`
3. `echo "localhost ansible_connection=local" >> /etc/ansible/hosts`
4. `ansible-pull --extra-vars "fqdn_sshserver=my.ssh.example.com no_shell_pass=deploysecret" -U https://github.com/skosachiov/linux-remote-assistance/playbooks/server.yml`

### Client configuration (X11 display server, no Wayland support yet)
1. Logon to the client and get root access
2. Install packages \
   Ubuntu: `apt install git ansible` \
   RHEL 8, Centos 8, Oracle linux 8: `dnf install epel-release; dnf install git ansible-core`
3. `echo "localhost ansible_connection=local" >> /etc/ansible/hosts`
4. `ansible-pull --extra-vars "fqdn_sshserver=my.ssh.example.com no_shell_pass=deploysecret" -U https://github.com/skosachiov/linux-remote-assistance/playbooks/client.yml`

## Client uninstall (ansible automation, pull mode)

`ansible-pull -U https://github.com/skosachiov/linux-remote-assistance/playbooks/client-uninstall.yml`

## Operation through port 443 through an existing HTTPS server, port 22 is closed on the firewall

### HTTPS server configuration

a2enmod ssl \
a2enmod proxy_connect

Add to coniguration /etc/apache2/sites-enabled/default-ssl.conf

`ProxyRequests On` \
`AllowCONNECT 22`

### Add to ssh connection command:

`apt/dnf install proxytunnel`

`ssh ... -o ProxyCommand="proxytunnel -z -E -p {{fqdn_sshserver}}:443 -d 127.0.0.1:22" ...`

## Screenshots

![ra-screenshot-00](https://github.com/skosachiov/linux-remote-assistance/raw/main/docs/screenshots/ra-screenshot-00.jpg)

![ra-screenshot-01](https://github.com/skosachiov/linux-remote-assistance/raw/main/docs/screenshots/ra-screenshot-01.jpg)

![ra-screenshot-02](https://github.com/skosachiov/linux-remote-assistance/raw/main/docs/screenshots/ra-screenshot-02.jpg)

![ra-screenshot-10](https://github.com/skosachiov/linux-remote-assistance/raw/main/docs/screenshots/ra-screenshot-10.jpg)

## A note on security

Obviously, we can set a legitimate ssh-server fingerprint on each workstation and not use the StrictHostKeyChecking=no option.
