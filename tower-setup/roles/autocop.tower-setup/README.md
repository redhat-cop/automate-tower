tower-setup
===========

This role allows to remotely setup or upgrade an Ansible Tower server, it has been tested with Tower 3.2 and 3.3 (I suspect that it would work down to Tower 3.0, but there is no guarantee, and why?).

The role should work without any specific variable defined, but you probably want to overwrite the default passwords (see the role variables below), and you can use variables to do such thing as:

- not download the latest version but a specific one
- grab the tarball from your own web server
- upgrade certain packages (or not) prior to setup
- ignore the check on the /var directory
- create a backup prior to upgrading
- adapt the working directory

The role can today only be used for a single tower installation with embedded database (i.e. no cluster, no external database). It is mostly idempotent as long as the setup.sh of Tower installation is (which it is today), even if the preparation steps might show some changed tasks (unpacking the tarball and patching the inventory).

The role works ok in check mode (as long as a setup directory has been unpacked).

Requirements
------------

A properly running RHEL 7 server with the pre-requisites defined in the installation handbook of Tower. CentOS 7 should work as well but hasn't been tested.

The role has been written in such a way that a normal user should be able to run most of the tasks, and only become a root where absolutely necessary, but it needs to be or be able to become root for certain tasks.

Role Variables
--------------

Following variables are defined as defaults and can be overriden to influence the way installation/upgrade of Ansible Tower is done.

which version of Ansible Tower to download and install/upgrade (e.g. 3.2.1-1):

	ansible_tower_version: latest

The name of the setup bundle and of the extracted directory (without el7):

	tower_base_name: "ansible-tower-setup-bundle-{{ ansible_tower_version }}"

Where to grab the bundle from:

	tower_base_url: "https://releases.ansible.com/ansible-tower/setup-bundle"

Where to download and unpack the bundle file (it will _not_ get removed!)

	tower_working_dir: "/root"

A hash of passwords for Tower, it can be (and should) of course overwritten by a vaulted variable:

	tower_passwords:
	  admin_password: changeme
	  pg_password: changeme
	  rabbitmq_password: changeme

Set to true if you want to skip on disk space in /var (at your own risk)

	tower_ignore_var_space: false

List of software packages to install or upgrade before installing / upgrading Tower

	tower_sw_packages:
	  - ansible
	  - python2-ansible-tower-cli

If you want to backup Tower prior to an upgrade, you need to define those two variables:

1. Final destination directory for the backup file

	tower_backup_dest: None

2. Temporary directory to create the backup

	tower_backup_dir: None

Dependencies
------------

There are no known dependencies. 

Example Playbook
----------------

An inventory could look as follows:

	all:
	  children:
	    tower_servers:
	      hosts:
		tower.example.com:
	      vars:
		ansible_user: root
		tower_passwords:
		  admin_password: myownpassword
		  pg_password: myownpassword
		  rabbitmq_password: myownpassword

Which would allow to use a playbook as easily as that:

	- hosts: tower\_servers
	  roles:
	  - { role: autocop.tower-setup }

License
-------

GPLv3+

Author Information
------------------

Check with the Automation Community of Practice at Red Hat,
e.g. https://github.com/redhat-cop/
