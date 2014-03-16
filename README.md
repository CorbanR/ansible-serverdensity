Ansible-Serverdensity
====================

[Ansible](http://www.ansibleworks.com/) Playbook for deploying the Server Density Agent

### Requirements 
[Ansible](http://docs.ansible.com/intro_installation.html) >= 1.5

### Platforms

* Ubuntu
* CentOS

## Usage

This will create a new device, and then use the agent key provided automatically by the API to configure the agent on the node.

Create an API token by logging into your Server Density account, clicking your name top left, clicking Preferences then going to the Security tab.

You will need to edit the following two files.

* vars/vars.yml
* inventories/hosts

### vars/vars.yml
```
# Required variables, please update
sd_url: "https://example.serverdensity.io"
api_token: "your_api_key"


#Optional adavanced configuration. Leave blank for defaults.

plugin_directory: ""
apache_status_url: ""
apache_status_user: ""
apache_status_pass: ""
fpm_status_url: ""
mongodb_server: ""
mongodb_dbstats: ""
mongodb_replset: ""
mysql_server: ""
mysql_user: ""
mysql_pass: ""
nginx_status_url: ""
rabbitmq_status_url: ""
rabbitmq_user: ""
rabbitmq_pass: ""
tmp_directory: ""
pidfile_directory: ""
logging_level: ""
```
### inventories/hosts

see hosts.example for more info

```
#Example with optional extra parameters (see ansible documentation: http://docs.ansible.com/intro_inventory.html#hosts-and-groups) 
[hosts]
hostname           ansible_ssh_user=user     ansible_ssh_port=22         ansible_ssh_host=ip_of_host            ansible_ssh_private_key_file=path_to_private_key

[hosts:vars]
groupname=sd_group_nam
#centos or debian
linux_distro=debian

```


### Optional Parameters

There are some optional parameters that can be used to configure other parts of the agent

* `plugin_directory:` -  Sets the directory the agent looks for plugins, if left blank it is ignored
* `apache_status_url:` - URL to get the Apache2 status page from (e.g. `mod_status`), disabled if not set
* `apache_status_user:` - Username to authenticate to the Apache2 status page, required if `apache_status_url` is set
* `apache_status_pass:` - Password to authenticate to the Apache2 status page, required if `apache_status_url` is set
* `fpm_status_url:` - URL to get the PHP-FPM status page from, disabled if not set
* `mongodb_server:` - Server to get MongoDB status monitoring from, this takes a full [MongoDB connection URI](http://docs.mongodb.org/manual/reference/connection-string/) so you can set username/password etc. details here if needed, disabled if not set
* `mongodb_dbstats:` - Enables MongoDB stats if `true` and `mongodb_server` is set, *default*: `false`
* `mongodb_replset:` - Enables MongoDB replset stats if `true` and `mongodb_server` is set, *default*: `false`
* `mysql_server:` - Server to get MySQL status monitoring from, disabled if not set
* `mysql_user:` - Username to authenticate to MySQL, required if `mysql_server` is set
* `mysql_pass:` - Password to authenticate to MySQL, required if `mysql_server` is set
* `nginx_status_url:` - URL to get th Nginx status page from, disabled if not set
* `rabbitmq_status_url:` - URL to get the RabbitMQ status from via [HTTP management API](http://www.rabbitmq.com/management.html), disabled if not set
* `rabbitmq_user:` - Username to authenticate to the RabbitMQ management API, required if `rabbitmq_status_url` is set
* `rabbitmq_pass:` - Password to authenticate to the RabbitMQ management API, required if `rabbitmq_status_url` is set
* `tmp_directory:` - Override where the agent stores temporary files, system default tmp will be used if not set
* `pidfile_directory:` - Override where the agent stores it's PID file, temp dir (above or system default) is used if not set
