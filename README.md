ansible-serverdensity
====================

[Ansible](http://www.ansibleworks.com/) playbook for automatically deploying the Server Density agent. 

## Requirements
[Ansible](http://docs.ansible.com/intro_installation.html) >= 1.8

## Platforms
* Debian/Ubuntu
* Redhat/CentOS

## Usage
This playbook will install the Server Density agent, allowing for custom configuration and server grouping.

## Required Setup
Create an API token using [these directions](https://apidocs.serverdensity.com/#authentication).

Edit the following files:
* `roles/serverdensity/vars/main.yml`
* `inventories/hosts`
* `serverdensity.yml`

### Configuration Examples
There are many different ways to set or overwrite Ansible variables. These are only a few examples. For more information about setting variables, read the official [Ansible Documentation](http://docs.ansible.com/playbooks_variables.html).

Example Ansible command: `ansible-playbook -i inventories/hosts serverdensity.yml`

Example: Setting `group_name` via the `inventories/hosts` file:
```
[development]
hostname ansible_ssh_host=ip_of_host
[development:vars]
group_name=development_servers
```
Example: Setting `group_name` via a role parameter:
```
- hosts: development
  gather_facts: yes
  sudo: yes
  roles:
    - { role: serverdensity, group_name: "development_servers" }
```
Example: Setting `group_name` and other variables via playbook variables:
```
- hosts: development
  gather_facts: yes
  sudo: yes
  vars: 
    group_name: "development_servers"
    mysql_server: "localhost"
    mysql_user: "root"
    mysql_pass: "password"
    
  roles:
    - serverdensity
```

## Default Variables
You are not required to update these variables in order to get the Server Density agent running. However, you can overwrite these variables in your playbook should you choose to do so.
```
group_name: "ungrouped"
plugin_directory: ""
apache_status_url: "http://www.example.com/server-status/?auto"
apache_status_user:
apache_status_pass:
fpm_status_url: ""
mongodb_server: ""
mongodb_dbstats: "no"
mongodb_replset: "no"
mysql_server: ""
mysql_user: ""
mysql_pass: ""
nginx_status_url: "http://www.example.com/nginx_status"
rabbitmq_status_url: "http://www.example.com:55672/json"
rabbitmq_user: "guest"
rabbitmq_pass: "guest"
tmp_directory: ""
pidfile_directory: ""
logging_level: ""
```

#### Default Variable Descriptions
* `group_name` - Sets the group name.
* `plugin_directory` -  Sets the directory where the agent looks for plugins. If the variable is left blank, it is ignored.
* `apache_status_url` - URL from which to retrieve the Apache2 status page (e.g. `mod_status`).
* `apache_status_user` - Username for Apache2 status page authentication. This value is required if `apache_status_url` is set.
* `apache_status_pass` - Password for Apache2 status page authentication. This value is required if `apache_status_url` is set
* `fpm_status_url` - URL from which to retrieve the PHP-FPM status page. This functionality is disabled if the variable is not set.
* `mongodb_server` - Server from which to retrieve MongoDB status monitoring information. This takes a full [MongoDB connection URI](http://docs.mongodb.org/manual/reference/connection-string/) so you can set any required details (e.g. username, password, etc.) as needed. This functionality is disabled if the variable is not set.
* `mongodb_dbstats` - Enables MongoDB stats if set to `yes` and a value for `mongodb_server` is also present.
* `mongodb_replset` - Enables MongoDB replset stats if set to `yes` and a value for `mongodb_server` is also present.
* `mysql_server` - Server from which to retrieve MySQL status monitoring information. This functionality is disabled if the variable is not set.
* `mysql_user` - Username for MySQL authentication. This variable is required if `mysql_server` is set.
* `mysql_pass` - Password for MySQL authentication. This variable is required if `mysql_server` is set
* `nginx_status_url` - URL from which to retrieve the Nginx status page.
* `rabbitmq_status_url` - URL from which to retrieve the RabbitMQ status via its [HTTP management API](http://www.rabbitmq.com/management.html).
* `rabbitmq_user` - Username for RabbitMQ management API authentication. This variable is required if `rabbitmq_status_url` is set.
* `rabbitmq_pass` - Password for RabbitMQ management API authentication. This variable is required if `rabbitmq_status_url` is set.
* `tmp_directory` - Override where the agent stores its temporary files. The default tmp directory will be used otherwise.
* `pidfile_directory` - Override where the agent stores its PID file. The `tmp_directory` value (which is either set explicitly or uses the system default) will be chosen otherwise.
