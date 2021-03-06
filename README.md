# ec2ansible: EC2 inventory generator for Ansible

[![Build Status](https://travis-ci.org/hehachris/ec2ansible.svg?branch=master)](https://travis-ci.org/hehachris/ec2ansible) [![Downloads](https://img.shields.io/pypi/dm/ec2ansible.svg)](https://pypi.python.org/pypi/ec2ansible) [![MIT License](https://img.shields.io/pypi/l/ec2ansible.svg)](https://github.com/hehachris/ec2ansible)

`ec2ansible` is a cli tool written in Python (2.7+) for generating realtime AWS EC2 inventory for [Ansible](http://docs.ansible.com/).

There is an [official script](http://docs.ansible.com/intro_dynamic_inventory.html#example-aws-ec2-external-inventory-script) doing the similar thing. But usually when we are provisioning a server, we only care about its **role**, eg. web server, database server, proxy server, etc, instead of its availability zone, AMI ID, nor instance type. `ec2ansible` groups your EC2 servers into Ansible inventory hierarchically based on the `Role` tag associated with them.

## Use Case
Asumming we have two main types of servers in the us-east-1 region: web servers and worker servers. And we have totally 5 servers as follow:
- Apache
- Nginx reverse proxy
- HAProxy reverse proxy
- Gearman worker
- Celery worker

Firstly we will add a `Role` tag for each of them with the following values:
- web_apache
- web_proxy_nginx
- web_proxy_haproxy
- worker_gearman
- worker_celery

Here is a sample grouping generated by `ec2ansible`:

```
use1 (us-east-1)
└─── web
     │    web_apache
     └─── web_proxy
          │    web_proxy_nginx
          │    web_proxy_haproxy
└─── worker
          │    worker_gearman
          │    worker_celery
```

## Installation
`ec2ansible` is available via [pypi](https://pypi.python.org/pypi/ec2ansible) and can be installed with easy_install or [pip](https://pip.pypa.io/en/latest/index.html):
```bash
pip install ec2ansible
```

## Usage
1. Save [ec2ansible-sample.ini](/hehachris/ec2ansible/blob/master/ec2ansible-sample.ini) to `~/.ec2ansible` and modify it if needed
2. Specify `/usr/local/bin/ec2ansible` as the inventory for Ansible

```bash
ansible web_apache -i /usr/local/bin/ec2ansible -u ec2-user -m ping
```
or set in `ansible.cfg`
```
hostfile = /usr/local/bin/ec2ansible
```
