# Archival Purposes

# WARNING - This playbook is outdated and no longer works on modern RHEL-Like distros.

This an archive of an Ansible playbook that used to work circa 2017 or so. It only targets CentOS 7 and installs the Nagios monitoring server, as well as installing the NRPE client on CentOS 7 and Ubuntu clients.

# Deploy Nagios Server

This repo contains an Ansible playbook that will automatically install and configure a CentOS 7 system as a Nagios monitoring server.

It will also install and configure the NRPE monitoring client on the nodes that you specify in the `monitored-servers` hostgroup. They will be configured to talk to the Nagios master server that you specify in the `nagios-server` hostgroup.

## Table of Contents
  * [Requirements](#requirements)
  * [Usage](#usage)

<a name="requirements"></a>
## Requirements

  * [Ansible Control System Requirements](https://docs.ansible.com/ansible/latest/intro_installation.html#control-machine-requirements)
  * A system with the `ansible` and `git` packages installed.
  * The ability to SSH to the target system as root with an SSH key.
  * The target system that will be the Nagios server needs to be running the CentOS 7 Linux distribution.
  * The target CentOS 7 system should have a valid FQDN (Fully Qualified Domain Name) on your local network.

<a name="usage"></a>
## Usage

This is a standalone playbook, meaning that it includes its own `ansible.cfg` file and does not need a system-level config file.

To use this playbook:

1. On your "control" system with ansible and git installed, clone this git repo.
  
2. In the [hosts](hosts) file, you will want to define the host that you want to make your nagios master server as well as the hosts that you want to monitor with said master server.
  * Define your nagios master server in the `nagios-server` hostgroup
  * Define your clients that you want to monitor in the `monitored-servers` hostgroup
  
3. In the [deploy-nagios-environment.yml](deploy-nagios-environment.yml) file, configure the following variables:
  * The `admin_name_01` variable. This is the name of the user that will be the default contact template for alerts.
  * The `nagios_username` variable. This is the username you will log into the web interface with.
  * The `admin_email_01` variable. This is the email address that you want to send alerts to.
  * The `nagios_password` variable. This is the password for the nagios web interface login.

4. Now, run the ansible deploy command with this playbook:

```
ansible-playbook deploy-nagios-environment.yml
```

In a few minutes, you should have a working Nagios monitoring server that is set up to monitor your defined clients.

After the playbook is finished, you can access the web interface of your Nagios server by navigating to:

`https://your-hostname.example.com/`

To login, use the credentials that you defined in the variables in the [deploy-nagios-environment.yml](deploy-nagios-environment.yml) file.

This installation uses a self-signed SSL cert by default, so your browser will warn you that it can't validate the cert and that it may not be secure. Just continue through to the login page.