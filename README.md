# Foreman Spark

## NOTE: THIS IS NOT FOR PRODUCTION (yet)

This repository contains the alternative, light-weight, opinionated installer
for Foreman and some plugins, that helps with setting up day 2 operations.

## Usage

1. Install git and ansible, e.g. `yum install -y git ansible`
1. Clone this git repository and cd into the dir, e.g. `git clone https://github.com/ares/foreman-spark; cd foreman-spark`
1. `ansible-playbook -c local deploy.yml`

## Installation description

It installs Foreman, Foreman Proxy and Ansible plugin. PostgreSQL is used
for DB backend. Foreman is running as a standalone service using Puma, which
listens on port localhost:3000. It also deploys Apache and configures proxy
pass to this Foreman instance. As part of the installation, self-signed
certificates are generated and configured.

## Known issues

### Can't connect to Foreman UI / API

Double check you have enabled https (TCP port 443) on your firewall,
the installer does not touch your firewall configuration. On FirewallD
systems you, following should do

```
firewall-cmd --zone=public --add-service=https
firewall-cmd --zone=public --add-service=https --permanent
```

### Certificate does not match my Foreman host name

Most likely you FQDN is misconfigured. Common trick to workaround this is to
edit /etc/hosts and add your desired FQDN to the first 127.0.0.1 line

```
127.0.0.1       foreman.example.com
```

### ERROR! Invalid callback for stdout specified: yaml

You use too old ansible. Edit ansible.cfg and comment out stdout_callback line

`#stdout_callback = yaml`

