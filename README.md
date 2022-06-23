# linuxserver_test
Ansible playbook to setup then break a LAMP server for testing basic Linux skills. Tested on Centos 7, AlmaLinux 8, and Debian 11.

### This is a work in progress. The script may have bugs and is not optimized, especially in how to handle failures. 

#### Installs
- Apache
- PHP 7.3
- Nginx (as a reverse proxy)
- Mariadb 5.5
- WordPress (via wp cli)

Service installations are idempotent.

#### How it breaks things
1. Changes the port Apache is Listening on (as an ingress from Nginx)
4. Sets a root password for MariaDB to prevent passwordless login
5. The mariadb service is stopped and not enabled to start on boot
9. Configures wp-config.php to use the wrong database connection details
10. wp-config.php is set to immutable
11. chmod 000 /usr/bin/ls
12. Sets iptable chain policies to DROP (except for SSH)

### Usage

The playbook needs to be used with *one* of two tags:

- provision : This tag assumes a fresh server. The playbook will install all services mentioned above then make all breaking changes.

- break : This tag assumes all services are already installed, though not necessarily running/working. The playbook will reapply all breaking changes as necessary.