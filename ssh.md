# SSH

- About
- SSH Daemon
- SSH-agent
- SSH Client
- SSH Server
- Connect via SSH
- Debugging and logging

## About

SSH - Secure Socket Shell - Secure Shell: is a **cryptographyc network protocol**

## SSH daemon

**sshd** is a program that runs as a background process. The letter _d_ in sshd stands for **daemon**.  
It constantly listens to TCP/IP port for client connection requests.

## SSH-agent

- About
- Set up

### About

Helper program for sign in. It will require the user to sign in only once.  
SSH-Agent opens up a socket over which the client and the user can exchange the signed data.

### Set up

`eval "$(ssh-agent -s)"` => will output the following: **Agent pid xxxx**

Adding a key:

`ssh-add ~/.ssh/id_rsa`

## SSH Client

The SSH client needs to be installed on the device you want to connect **from**. It is installed by default on Linux.

To install the open source ssh client:  
`sudo apt-get install openssh-client`

## SSH Server

- About
- Install
- Available commands

### About

The SSH Server will allow the device to be connected **to** like a server. It is not installed by default on Linux.

### Install

Install:  
`sudo apt-get install openssh-server ii.`

### Available commands

`sudo service ssh status` => will output the current status of the SSh Server  
`sudo service ssh start` => will start the SSH Server  
`sudo service ssh restart` => will restart the SSH Server  
`sudo service ssh try-restart` => will try to restart the SSH Server  
`sudo service ssh stop` => will stop the SSH Server  
`sudo service ssh reload` => will reload the SSH Server  
`sudo service ssh force-reload` => will force reload the SSH Server

## Connect via SSH

There are two ways to connect onto a remote system:

- password authentication
- public key authentication

## password authentication

## public key authentication

### Check for existing key:

`ls -al ~/.ssh/id_*.pub`

### Generate SSH Key Pair:

`ssh-keygen -t rsa -b 4096`

By default the **identification key** or **private key** will be stored in /home/userName/ssh/id_rsa.  
The **public key** will be stored in /home/userName/ssh/id_rsa.pub

### Uploading the key to the server

The server needs to have the client's public key to identify it.  
Copy the key from the client to server:  
`ssh-copy-id userName@remoteIP` i.e: `ssh-copy-bob@192.168.x.x`

The authorized keys will be stored in: /home/userName/.ssh/authorized_keys

### Connecting Locally

`ssh username@host_ip_address` : will connect to a server on a local network

### Connecting Remotely

`ssh userName@remoteIpAddress` : will connect to a remote server like an internet box.
`ssh pi@[public ip of the bbox]`

## Debugging and logging

There are three levels of verbosity to display info. Add the following to the ssh commands:

- -v (level 1)
- -vv (level 2)
- -vvv (level 3)
