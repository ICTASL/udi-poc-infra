# MOSIP Multi-VM Sandbox Installer

## Introduction

The folders here contain Ansible scripts to run MOSIP on a multi Virtual Machine (VM) setup.  

* `kube/`:  Scripts to install Kubernetes
* `app/`:  Scripts to install all MOSIP modules


## Hardware setup 

The following VMs are recommended:

### Kubernetes node VMs
1. Kubernetes master:  1 (4 CPU, 16 GB RAM)
1. Kubernetes workers:  n (4 CPU, 16 GB RAM)

* n = 2 for Pre Reg only
* n = 4 for Pre Reg + Reg Proc
* n = 5 for Pre Reg + Reg Proc + IDA

All the above in the same network.

### Console
Console machine: 1 (2 CPU, 8 GB RAM) 

## Console setup
Console machine is the machine from where you will run all the scripts.  The machine needs to be in the same network as all the Kubernetes nodes.  Your Ansible scripts run on the console machine. You may work on this machine as non-root user.
* Change hostname of console machine to `console`. 
* Create a (non-root) user account on console machine.
* Make `sudo` password-less for the user.
* Create ssh keys using `ssh-keygen` and place them in ~/.ssh folder:
* Login as root with `sudo su` write keys in authorized_keys

```
$ ssh-keygen -t rsa
```
No passphrase, all defaults.
* Include current user in /etc/sudoers file with no password. 
* Install Ansible
```
$ sudo yum install -y https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
$ sudo yum install ansible # This will install 2.9 version
```
* Install git:
```
$ sudo yum install -y git
```
* Git clone this repo in user home directory.

* Set root user password, say `rootpassword`.

## K8s cluster machines setup
* Set up kubernetes machines with following hostnames matching names in hosts.ini. (may require reboot of machines)
* If you have more nodes in the cluster add them to `hosts.ini`.   
* Enable root login to all the machines with same password as above `rootpassword`.

## Running Ansible scripts
```
$ ansible-playbook -i hosts.ini site.yml

To run individual roles, use tags, e.g
```
$ ansible-playbook -i hosts --tags postgres site.yml
```
