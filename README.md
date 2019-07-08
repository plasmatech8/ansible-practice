# Ansible

* Playbooks
	* Plays
		* Tasks
			* Call Modules

Tasks are run sequencially.

Handlers can be triggered by a Task and are ryb once at the end of a Play.

## Example
```yaml
---
- name: installs and start apache
  hosts: web
  remote_user: justin
  become_method: sudo
  become_user: root
	
  vars:
      http_port: 80
      max_clients: 200

  tasks:
  - name: install httpd
    yum: name=httpd state=latest
  - name: write apache config file
    template: src=srv/httpd.j2 dest=/etc/httpd.conf
    notify:
    - restart apache
  - name: start httpd
    service: name=httpd state=running
			
  handlers:
  - name: restart apache
    service: name=httpd state=restarted

```

* --- signifies that it is a yaml file.
* playlist name is `installs and start apache`
* Some directives are set
* `hosts` specifies which inventory group to apply this playbook to
* `remote_user` is the user to use for remove connections
* `become_method` and `become_user` escalates priveledges (sudo/root mode)
* Variables are used
* Tasks are run sequencially
	* Name
	* Module (how to configure the target system)
	* We can deploy a configuration file using a template module 
		* The variables in a template module will be replaced with the variables in the playlist
	* We can ensure that the service is started with service
	
## Modules
There are over 450 Ansible-provided modules that automate nearly every part of your environment.

http://docs.ansible.com/ansible/modules_by_category.html

```yaml
module: directive1=value directive2=value
```

## Roles

A special kind of playbook that is ful self contained with tasks, variables, configuration templates and other supporting files.

## How to run

1. Call a module from command line (ad-hoc)

```
e.g.
ansible <inventory> <options>
ansible web -a /bin/date
ansible web -m ping
ansible web -m yum -a "name=openssl state=latest"
```
2. Call a playbook from command line
3. Using Ansible Tower

