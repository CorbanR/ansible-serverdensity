ansible-serverdensity
====================

[Ansible](http://www.ansibleworks.com/) playbook for automatically deploying the server density agent. 

## Requirements
[Ansible](http://docs.ansible.com/intro_installation.html) >= 1.8

## Platforms

* Debian/Ubuntu
* Redhat/CentOS

## Usage

This playbook will install the server density agent, allowing for custom configuration and server grouping. 

## Initial Setup
Create an API token using these directions 
[https://apidocs.serverdensity.com/#authentication](https://apidocs.serverdensity.com/#authentication)

You will need to edit the following two files.

* `roles/serverdensity/vars/main.yml`
* `inventories/hosts`

### Examples
Example 1: Setting `group_name` via hosts file `inventories/hosts`
```
[development]
hostname ansible_ssh_host=ip_of_host
[development:vars]
group_name=development_servers
```
Example 2: Setting `group_name` via playbook
```
- hosts: development
  gather_facts: yes
  sudo: yes
  roles:
    - { role: serverdensity, group_name: "development_servers" }
```
Example 3: Setting `group_name` and other varaibles via playbook version 2
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

## Default variables
You are not requried to update these in order to get the server density agent to run. You can overwrite these variable in your playbook. See the above examples on how to do so. 
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

#### Default variable descriptions
* `group_name` - Sets the group name 
* `plugin_directory:` -  Sets the directory the agent looks for plugins, if left blank it is ignored
* `apache_status_url:` - URL to get the Apache2 status page from (e.g. `mod_status`)
* `apache_status_user:` - Username to authenticate to the Apache2 status page, required if `apache_status_url` is set
* `apache_status_pass:` - Password to authenticate to the Apache2 status page, required if `apache_status_url` is set
* `fpm_status_url:` - URL to get the PHP-FPM status page from, disabled if not set
* `mongodb_server:` - Server to get MongoDB status monitoring from, this takes a full [MongoDB connection URI](http://docs.mongodb.org/manual/reference/connection-string/) so you can set username/password etc. details here if needed, disabled if not set
* `mongodb_dbstats:` - Enables MongoDB stats if `yes` and `mongodb_server` is set
* `mongodb_replset:` - Enables MongoDB replset stats if `yes` and `mongodb_server` is set
* `mysql_server:` - Server to get MySQL status monitoring from, disabled if not set
* `mysql_user:` - Username to authenticate to MySQL, required if `mysql_server` is set
* `mysql_pass:` - Password to authenticate to MySQL, required if `mysql_server` is set
* `nginx_status_url:` - URL to get th Nginx status page from
* `rabbitmq_status_url:` - URL to get the RabbitMQ status from via [HTTP management API](http://www.rabbitmq.com/management.html)
* `rabbitmq_user:` - Username to authenticate to the RabbitMQ management API, required if `rabbitmq_status_url` is set
* `rabbitmq_pass:` - Password to authenticate to the RabbitMQ management API, required if `rabbitmq_status_url` is set
* `tmp_directory:` - Override where the agent stores temporary files, system default tmp will be used if not set
* `pidfile_directory:` - Override where the agent stores it's PID file, temp dir (above or system default) is used if not set
