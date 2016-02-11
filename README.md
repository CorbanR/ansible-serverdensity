ansible-serverdensity
====================
[Ansible](http://www.ansibleworks.com/) playbook for automatically deploying the Server Density agent.

## Requirements
[Ansible](http://docs.ansible.com/intro_installation.html) >= 1.9

## Platforms
* Debian/Ubuntu
* Redhat/CentOS

## Usage
This playbook will install the Server Density v2 agent, allowing for custom configuration and server grouping.

## Whats new
- Now installs Server Density agent v2.
- Added upgrade logic.
- Added options for Server Density plugin installation.
- Updated syntax (yaml)

## Required Setup
Create an API token using [these directions](https://apidocs.serverdensity.com/#authentication).

Edit the following files:
* `roles/serverdensity/vars/main.yml` #Required
* `inventories/hosts` #Required
* `serverdensity.yml`
* `roles/serverdensity/templates/conf.d/*` #Only need to update if you choose to install official plugins.

## Upgrade v_1 to v_2
To Upgrade v_1 to v_2 set following variable `upgrade_v2: yes`.

If this is a fresh install do NOT set the variable.

### Configuration Examples
There are many different ways to set or overwrite Ansible variables. These are only a few examples. For more information about setting variables, read the official [Ansible Documentation](http://docs.ansible.com/playbooks_variables.html).

Example Ansible command: `ansible-playbook -i inventories/hosts serverdensity.yml`

Example Ansible command(Upgrade): `ansible-playbook -i inventories/hosts serverdensity.yml --extra-vars "upgrade_v2=yes"`

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
    plugins:
      - apache
      - nginx
      - mongo
      - mysql
      - rabbitmq
  roles:
    - serverdensity
```

## Default Variables
You are not required to update these variables in order to get the Server Density agent running.
```
group_name: "ungrouped"
plugin_directory: ""
```

#### Default Variable Descriptions
* `group_name` - Sets the group name.
* `plugin_directory` -  Sets the directory where the agent looks for plugins. If the variable is left blank, it is ignored.
