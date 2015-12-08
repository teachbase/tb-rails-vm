## About

VM configuration for RoR development.

Installs:

* Ruby 2.1.0 (+ bundler)
* Nginx (configured to use with Puma)
* Postgres 9.3 (+ {{app_name}} user with CREATEDB priv)
* Redis
* NodeJS


## Requirements

* Vagrant >=1.1
* VirtualBox >=4.0
* Ansible (http://docs.ansible.com/intro_installation.html)

## Setup

* `ansible-galaxy install -r priv/ansible/requirements.yml`
* `vagrant up` that's all!

### Possible issues

* ```The guest additions on this VM do not match the installed version of
VirtualBox!```
	Solve with plugin: `vagrant plugin install vagrant-vbguest`