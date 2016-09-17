# Configuration for deploying Laravel Project using Ansible

## Prepare

* Please install `python-simplejson, apt-transport-https, ca-certificates` on **Remote server**:

    ```
    sudo apt-get install -y python-simplejson apt-transport-https ca-certificates
    ```

## Usage

### Config

* Add **Drone's** project public key to github/bitbucket (scm) **Deploy keys** and Remote server file: `~/.ssh/authorized_keys`
* Copy `.ansible` folder and `.drone.yml` to your Laravel folder.
* You have to change some configurations in the following files:
    * Ansible’s inventory file: `.ansible/production` and `.ansible/staging`
        * We have 2 servers: [webservers] and [dbservers] server, please change both of them (it can be the same)
        * Change IP address host, ssh user (`ansible_ssh_user`), ssh port (`ansible_ssh_port`)
    * All variables file: `.ansible/group_vars/all.yml`
        * **repository**: Github/Bitbucket (scm)
        * **branch**: Git branch name for deployment
        * **dbuser**: Database user
        * **dbname**: Database name
        * **dbpassword**: Database user's password
    * Web variables file: `.ansible/group_vars/webservers.yml`
        * **shared_items**: List all items inside `shared` folder
    * Database variables file: `.ansible/group_vars/dbservers.yml`

### Commands:

* For install `php, composer, mysql, nginx, npm, node, bower` and deploy

    ```
    ansible-playbook -i production site.yml --extra-vars='ansible_become_pass={become_password} init=yes'
    ```

    `{become_password}` is password of ssh user on server. It's Not Necessary, if ssh user is _root_

* For only deployment without setup anything (Make sure that `php, composer, mysql, nginx, npm, node, bower` is installed)

    ```
    ansible-playbook -i production site.yml --extra-vars='ansible_become_pass={become_password}'
    ```

* Modify commands in `.drone.yml` file.

_Note: you can remove MariaDB with command: `sudo apt-get --purge remove "mysql*"`_