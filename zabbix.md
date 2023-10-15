# Zabbix

- [About](#about)
- [Install](#install)
- [Uninstall](#uninstall)
- [Run server](#run-server)

## About

Zabbix: server monitoring

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

## Run server

`sudo service zabbix-server start`  
`sudo service zabbix-agent start`  
`sudo service apache2 start`
or
`sudo service apache2 start`

### Zabbix server

The **zabbix server** will collect the data from the **zabbix agent**.

### Zabbix agent

The **zabbix agent** will send the data from the **zabbix server**.

Monitors:

=> Windows Rabbix agent
=> Template

- the VPN
- console : transactions
- WI6LAB
- Gateway
- RabbitMQ

STS Meters (South African)
Cebata Senegal

ThingsMobile : SIM card roaming on GPRS meters
MVNO: mobile virtual network operator

=> NRW

- no data from Bulk meter since some date
- from Bulk meter since some date

=> Monitor RDS

- MySQL template
- Use the doc

WYOTIS = WI6LAB
