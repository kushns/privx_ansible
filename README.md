![Ansible code validation](https://github.com/kushns/privx_ansible/workflows/Ansible%20code%20validation/badge.svg)

# PrivX-Installation-Rocky-Linux

*This repository can used to install ***PrivX*** with **External PostgreSQL** on ***Rocky Linux 8 hosts****
 

## Pre-requisites

1.  Rocky Linux 8 hosts (2 or more) 
1.  Install [Git](https://git-scm.com/downloads)
1.  Install [Ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#installing-ansible-on-rhel-centos-or-fedora)
1.  **DNS Lookup**: Make sure that hosts can resolve/ping each other by name(update /etc/hosts file if required)
  
### Enable SSH to communicate all the servers with out "password".

##### Execute on host from which Ansible playbook will be executed
  
* ***Generate ssh key pair***

    `$ ssh-keygen -t rsa`

*   ***Copy key to target servers***

    `$ cat ~/.ssh/id_rsa.pub | ssh root@192.168.56.10  "mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys && chmod 600 ~/.ssh/authorized_keys"`
    
    `$ cat ~/.ssh/id_rsa.pub | ssh root@192.168.56.11  "mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys && chmod 600 ~/.ssh/authorized_keys"`


## Deployment

*   #### Clone privx_ansible repo
    ```
    $ git clone ttps://github.com/kushns/privx_ansible
    $ cd privx_ansible
    ```

*    #### Update Mandatory Variables in inventory.ini file
     ```
     # Update inventory.ini with IP addresses of privx, privxdb servers.
     # One or more ip addresses can be specified under privx host group if multinode setup required.  
     
     # host group for PrivX servers ip (one or more)
     [privx]
     192.168.56.11
     192.168.56.12

     # host group for PostgreSQL DB server ip
     [privxdb]
     192.168.56.10
     ```

*   #### Update Optional Variables in inventory.ini file (if required)
    ```
    # Postgresql version (Code tested for versions 15)
    PGSQL_VERSION=15
    
    # PostgreSQL database user name
    PRIVX_DATABASE_USERNAME=privx
    
    # Random password will be generated for database user specified above
    # and will be stored in ~/database_password file
    # Static password can be used by updating variable PRIVX_DATABASE_PASSWORD
    PRIVX_DATABASE_PASSWORD="mypassword"
    
    # PostgreSQL database name for PrivX, update if necessary
    PRIVX_DATABASE_NAME=privx
    
    # PrivX Superuser
    PRIVX_SUPERUSER=admin
    
    # Random password will be generated for privx user specified above
    # and will be stored in ~/privx_password file
    # Static password can be used by updating variable PRIVX_SUPERUSER_PASSWORD
    PRIVX_SUPERUSER_PASSWORD="mypassword"
    
    # Ansible remote user and become value, default set to root
    # Uncomment and supply REMOTE_USER value if remote user is not root
    REMOTE_USER=centos
    BECOME=yes
    ```
*   #### Run deploy.yml playbook to perform installation
    ```
    $ ansible-playbook -i inventory.ini deploy.yml
    ```

    **Once all the ansible jobs are successful, you can access PrivX GUI on https://<privx_server_hostname_or_IP>**
    
    **RANDOM password can be found in ~/privx_password**
    
    
*   #### To activate a PrivX license with the online method:
    1. Access the PrivX GUI and navigate to the Settings→License page.
    2. Under the License code section, enter your license code, and click Update License.
    
    
## Next Steps
 * [Getting Started with PrivX](https://privx.docs.ssh.com/docs)
