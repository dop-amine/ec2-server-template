## Overview

Ansible playbook template to deploy Ubuntu 18.04 server on AWS EC2 and locally with Vagrant

## Setup

1. Enter relevant variables in the `vars/vars.yml` file
1. Add your ssh public key(s) to `vars/ssh_keys/authorized_keys.yml`
1. Edit the config files in `vars/configs/`
1. Add any tasks you need to `tasks/`, `main.yml` and `local.yml`
1. Add any roles you need

When your site is created and has an associated domain name/DNS resource, add it below `[web]` in the `hosts` file.

## Web Usage

`lab/web/main.yml` is used to build and manage servers and dependencies.

```
ansible-playbook --ask-vault-pass -i hosts --key-file "~/devops/local/key_pairs/<keypair>.pem" main.yml
```

## Local Usage

**Vagrant Commands:**

* **Build:** `vagrant up`
* **Connect:** `vagrant ssh`
* **Reload:** `vagrant reload`
* **Re-provision:** `vagrant provision`
* **Stop:** `vagrant halt`
* **Delete:** `vagrant destroy -f`

**VM Tips & Tricks:**

* **View login message:** `motd`
* **File browser:** `ranger`
* **Reload dotfiles:** `sauce`
* **Netdata** real-time performance monitor in the browser at `my.winmo.vm:19999`
  * _follow troubleshooting steps [here](http://wiki.tldev2.com/books/devops/page/monitoring#bkmrk-troubleshooting) if Netdata is not loading_

**Resource Allocation:**

You can easily change resource allocation for the VM in the `lab/local/Vagrantfile`.

* `v.memory = <memory>` = Memory / RAM allocated to the VM
* `v.cpus = <cpu_cores>` = CPU Cores allocated to the VM

## Dependencies

#### Ansible

```
sudo easy_install pip
sudo pip install ansible
sudo mkdir /etc/ansible
sudo touch /etc/ansible/hosts
sudo touch /etc/ansible/ansible.cfg
```

## Vagrant

[Download and install from website.](https://www.vagrantup.com/downloads.html)

## Virtualbox

[Download and install from website.](https://www.virtualbox.org/wiki/Downloads)

If experiencing issues installing on mac, [follow this guide](http://wiki.tldev2.com/books/devops/page/%F0%9F%92%8E-the-lab-%F0%9F%92%8E#bkmrk-vagrant-up-and-virtu).

## AWS CLI

First, [create an AWS account](https://portal.aws.amazon.com/billing/signup#/start) if you haven't already.

Install pip3:

```
brew install python3
brew postinstall python3
```

If you encounter permissions issues do this:

```
sudo mkdir /usr/local/Frameworks
sudo chown $(whoami):admin /usr/local/Frameworks
```

Install aws-cli:

```
pip3 install awscli --upgrade --user
echo "export PATH=~/Library/Python/3.7/bin:$PATH" >> ~/.profile
sudo pip install boto
```

Go to AWS dashboard and create keys for your user.

Create ~/.aws directory and files:

```
mkdir ~/.aws
touch ~/.aws/config
touch ~/.aws/credentials
```

Add the text below to `~/.aws/config`:

```
[default]
region = us-east-2
```

Add the text below to `~/.aws/credentials` with relevant credentials:

```
[default]
aws_access_key_id = <aws access key>
aws_secret_access_key = <aws secret key>
```

## Using [Ansible Vault](https://docs.ansible.com/ansible/latest/user_guide/vault.html)

Make sure to encrypt any passwords/secrets with ansible vault:

* **Creating encrypted files:** `ansible-vault create file.yml`
* **Editing encrypted files:** `ansible-vault edit file.yml`
* **Encrypting files:** `ansible-vault encrypt file1.yml file2.yml file3.yml`
* **Decrypting files:** `ansible-vault decrypt file.yml`
