# Notes

https://www.braveclojure.com/quests/deploy/intro/

## Intro

- fully funcioning example all-clojure single-page app
- Ubuntu server with `nginx`, `java` and `Datomic Free`
- Datomic database
- *Ansible roles*
- Test on a *virtual machine* with *Vagrant*
- Host a Virtual Private Server (**VPS**) hosted by *Digital Ocean*

## Chapter 1 - Setup a server and deploy a Clojure App to it

> [Boot](https://github.com/boot-clj/boot#install)
> [Ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#installing-the-control-machine)
> [VirtualBox](https://www.virtualbox.org/wiki/Downloads)
> [Vagrant](https://www.vagrantup.com/)

```bash
# install boot
# if you have nix:
nix-env -i boot
# otherwise:
sudo bash -c "cd /usr/local/bin && curl -fsSLo boot https://github.com/boot-clj/boot-bin/releases/download/latest/boot.sh && chmod 755 boot"

# install ansible
sudo apt-get install software-properties-common
sudo apt-add-repository ppa:ansible/ansible
sudo apt-get update
sudo apt-get install ansible

# install VirtualBox
sudo apt-get install virtualbox

# install vagrant
sudo apt-get install vagrant
```

Getting the example repository

```bash
git clone https://github.com/sweet-tooth-clojure/character-sheet-example.git
cd character-sheet-example
# or
curl -LOks https://github.com/sweet-tooth-clojure/character-sheet-example/archive/master.zip
unzip master.zip
cd character-sheet-example/infrastructure
```

Install the Ansible *roles*

```bash
ansible-galaxy install -r ansible/requirements.yml
```

Create a Virtual Machine

```
vagrant up
```

This setups the VM with nginx and datomic and it should be ready to deploy the example app.

To **shutdown** the vagrant server: `vagrant halt`
To start from scratch: `vagrant destroy -f` then `vagrant up` (in `infrastructure/`)

Build and deploy to a VM

```bash
# in infrastructure/
./build && ./deploy dev
```

`build`:
- downloads the dependencies
- compiles clojurescript
- packs everything into an **uberjar**

`deploy dev`:
- copies the uberjar to the VM
- runs database migrations
- runs basic check that the app is functioning
- then starts the app as an ubuntu service

## Chapter 2

Why the deployment tools work and how they are setup

## Chapter 3

Different tools involved and how to use them individually and together
