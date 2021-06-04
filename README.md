Infra challenge
=========

A repository contains all project files for creating HA environment

Requirements
------------

- Download and install VirtualBox.
- Download and install Vagrant.
- [Mac/Linux only] Install Ansible.
- Had keys id_rsa and id_rsa.pub in your `~/.ssh/`
- Run `ansible-galaxy install -r requirements.yml` in this directory to get the required Ansible collections.
- Run `vagrant plugin install vagrant-docker-compose` to get the required vagrant plugin.

Role Variables
--------------

There is two files with variables `group_vars/workers.yaml` and `group_vars/proxy.yaml`

`group_vars/workers.yaml`:
- appImageName - a variable to specify image name for docker compose
- vmHostPort - a variable to specify the host port for forwarding
- containerPort - a variable to specify the container port for forwarding

`group_vars/proxy.yaml`:
- appImageName - a variable to specify image name for docker compose
- vmHostPort - a variable to specify the host port for forwarding
- containerPort - a variable to specify the container port for forwarding
- backendPort - a variable to specify the port for connecting to the backend

Dependencies
------------
Ansible:
- community.docker

Vagrant:
- docker-compose

Initialization
----------------

1. Run `vagrant up`
2. Run `ansible-playbook playbooks/main.yml -i environment/hosts.ini`
3. `curl 192.168.70.5`

To perform maintenance without interrupting user operations, was built architecture with 3 application servers and 1 proxy, you can do maintenance for any of servers and it is didn't affect user operations due to proxy.
The infrastructure is fairly simple, with the following structure:

                                              -----------------------
                                             | Nginx (192.168.70.5) |
                                              -----------------------
                                                        |
                         ________________________________________________________
                        |                               |                        |
              ----------------------         ----------------------      ----------------------
             | App (192.168.70.2)   |       | App (192.168.70.3)   |    | App  (192.168.70.4)  |
              ----------------------         ----------------------      ----------------------
