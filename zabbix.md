# Zabbix

- [About](#about)
- [Install](#install)
- [Uninstall](#uninstall)
- [Create database](#create-database)
- [Zabbix server](#zabbix-server)
- [Zabbix agent](#zabbix-agent)
  - [Zabbix Agent on different server](#zabbix-agent-on-different-server)
  - [Manually create the host](#manually-create-the-host)
  - [Auto registration of hosts](#auto-registration-of-hosts)
- [Enable PSK(Pre-Shared Key) encryption](#enable-pskpre-shared-key-encryption)

## Set up a server using Digital Ocean

- Create a droplet
- Choose the Ubuntu 20.04 image
- Choose the $5/mo plan
- Choose a datacenter region
- Add your SSH key
- Choose a hostname
- Click on Create Droplet
- Configure the Firewall

## About

Zabbix is an open-source monitoring software tool for diverse IT components, including networks, servers, virtual machines (VMs) and cloud services.  
Zabbix provides monitoring metrics, among others network utilization, CPU load and disk space consumption.

## Install

[Link => Zabbix download page](https://www.zabbix.com/download?zabbix=6.4&os_distribution=ubuntu&os_version=20.04&components=server_frontend_agent&db=mysql&ws=apache)

- Install Zabbix repository (for Ubuntu 20.04):

```bash
wget https://repo.zabbix.com/zabbix/6.4/ubuntu/pool/main/z/zabbix-release/zabbix-release_6.4-1+ubuntu20.04_all.deb
sudo dpkg -i zabbix-release_6.4-1+ubuntu20.04_all.deb
sudo apt update 
```

- Check if Zabbix was installed: `apt search zabbix-server`
- Install Zabbix server, frontend, agent: `apt install zabbix-server-mysql zabbix-frontend-php zabbix-apache-conf zabbix-sql-scripts zabbix-agent`  
- check status  
`sudo service zabbix-server status`  
`sudo service zabbix-agent status`  
- Create initial database. For that make sure MySQL is  running. Refer to the **sql.md** documentation.

## Uninstall

`sudo apt-get remove zabbix-server-mysql zabbix-frontend-php zabbix-apache-conf zabbix-sql-scripts zabbix-agent`

## Create database

In case MySQL is not installed:  
`apt install mysql-server` and check the status: `sudo service mysql status`

Create a database:

```mysql
mysql
create database zabbix character set utf8 collate utf8_bin;
create user zabbix@localhost identified by 'password';
grant all privileges on zabbix.* to zabbix@localhost;
set global log_bin_trust_function_creators=1;
quit;
```

Import the initial schema and data:

```bash
zcat /usr/share/zabbix-sql-scripts/mysql/server.sql.gz | mysql --default-character-set=utf8mb4 -uzabbix -p zabbix
```

Now to disable the log_bin_trust_function_creators option:

```mysql
mysql
set global log_bin_trust_function_creators=0;
quit;
```

Edit Zabbix Server configuration file and add the database password:

```bash
sudo nano /etc/zabbix/zabbix_server.conf
DBPassword=password # look for this line and add the password
```

## Zabbix server

The **zabbix server** will collect the data from the **zabbix agents**.

Start the Zabbix Server Process

```bash
sudo service zabbix-server start
sudo service zabbix-server status

# Enable the Zabbix Server and Agent to start on boot
sudo enable zabbix-server zabbix-agent apache2
```

## Zabbix agent

The **zabbix agent** will send the data to the **zabbix server**.

### Zabbix Agent on different server

Update the Zabbix Agent configuration file:

```bash
sudo nano /etc/zabbix/zabbix_agentd.conf
Server=Zabbix-Server-IP # replace Zabbix-Server-IP with the IP address of the Zabbix Server
ServerActive=Zabbix-Server-IP # replace Zabbix-Server-IP with the IP address of the Zabbix Server
Hostname=Zabbix-Agent-Hostname # replace Zabbix-Agent-Hostname with the hostname of the Zabbix Agent

# Restart the Zabbix Agent
sudo service zabbix-agent restart
```

### Manually create the host

In the Zabbix Web Interface, go to Data collection -> Hosts -> Create Host.

- Host name: hostname where the Zabbix Agent is installed. (run `hostname` in the terminal)
- Templates: e.g: **Linux by Zabbix Agent** or **Linux by Zabbix Agent Active**
- Groups: e.g: **Linux servers**
- Agent interfaces: IP address of the Zabbix Agent
- Click on Add

### Auto registration of hosts

- Go to Alerts -> Actions -> Autoregistration actions
- Create a new action
- Conditions: Host metadata contains: [value], e.g: 'linux'
- Operations:
  - Add host
  - Add to host groups: [value], e.g: 'Linux servers'
  - Link to templates: [value], e.g: 'Linux by Zabbix Agent'

## Enable PSK(Pre-Shared Key) encryption

PSK encryption is used to encrypt the data between the Zabbix Server and the Zabbix Agent.

```bash
# Create a directory to store the secret key
mkdir /home/zabbix/ && cd /home/zabbix/
sudo openssl rand -hex 32 > secret.key

# Change the ownership of the file to the zabbix user
sudo chown zabbix:zabbix secret.key

# Change the permissions of the file
sudo chmod 640 secret.key

# Update the Zabbix Agent configuration file
sudo nano /etc/zabbix/zabbix_agentd.conf

# Add the following lines to the configuration file
TLSConnect=psk
TLSAccept=psk
TLSPSKFile=/home/zabbix/secret.key
TLSPSKIdentity=[PSK-Identity] # e.g: PSK-Identity=Host-A

# Restart the Zabbix Agent
sudo service zabbix-agent restart
```

On the Zabbix interface update the encrytion details of the host: 

- Go to Data Collection -> Hosts -> Host => Encryption
- Connections to host: PSK
- Connections from host: PSK
- PSK identity: [same value as the TLSPSKIdentity]

