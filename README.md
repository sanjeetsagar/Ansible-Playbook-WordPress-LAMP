# Ansible WordPress for a LEMP server

## Background

Vagrant and VirtualBox (or some other VM provider) can be used to quickly build or rebuild virtual servers.

This Vagrant profile installs Linux, Apache, MySQL, and PHP (LAMP Stack) using the [Ansible](http://www.ansible.com/) provisioner.

## Getting Started

This README file is inside a folder that contains a `Vagrantfile`, which tells Vagrant how to set up your virtual machine in VirtualBox.

To use the vagrant file, you will need to have done the following:

1. Download and Install [VirtualBox](https://www.virtualbox.org/wiki/Downloads)
2. Download and Install [Vagrant](https://www.vagrantup.com/downloads.html)
3. Open a shell prompt (Terminal) and cd into the folder containing the `Vagrantfile`

Once all of that is done, you can simply type in `vagrant up`, and Vagrant will create a new 3 VM, `master`, `node1` and `node2`. From master VM we use Ansible Playbook

Once the new VM is up and running (after `vagrant up` is complete and you're back at the command prompt), you can log into it via SSH if you'd like by typing

```
vagrant ssh master
```

Now go to playbook directory `cd /home/vagrant/playbook` Run the following command to install the necessary Ansible roles for this profile:

```
ansible-galaxy install -r requirements.yml
```

Run WordPress LAMP

```
ansible-playbook wordpress.yml
```

### Setting up your hosts file

You need to modify your host machine's hosts file (Mac/Linux: `/etc/hosts`; Windows: `%systemroot%\system32\drivers\etc\hosts`), adding the line below:

    192.168.33.31 master.test
    192.168.33.32 node1.test
    192.168.33.33 node2.test

(Where `lAMP`) is the hostname you have configured in the `Vagrantfile`).

After that is configured, you could visit http://node1.test/ or http://node2.test/ (depend on playbook) in a browser, and you'll see the WordPress is running.
