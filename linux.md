# Linux

- [Basic commands](#basic-commands)
- [Permissions](#permissions)
- [bash_aliases](#bash_aliases)
- [Kill WSL2](#kill-wsl2)
- [Restart WSL2](#restart-wsl-2)
- [Kill node process](#kill-node-process)

## Basic commands

- [Display files and directory](#display-files-and-directory)
- [cd](#cd)
- [grep](#grep)
- [Search command history](#search-command-history)
- [Count the number of files in directory](#count-the-number-of-files-in-directory)

### Display files and directory

`ls` => will list the files and folders in the required directory  
`ls -a` => will list all files and folders including the hidden ones (starting with **.**, e.g: .bash_alias)
`ls -l` => will list the files and folders with informations like owners, permissions and size
`ll` => same as `ls -l` but will also display hidden files

### cd

**change directory** => will navigate to the desired directory:  
`cd [directoryNamePath]`  
`cd ../` => will move up one level  
`cd /` => will navigate to the home directory  
`cd ~` || `cd` => will navigate to the user directory
`cd -` => will go back to the previous location

### grep

Will output the desired keyword of a given file if any:

`grep [keyword] [/path/to/file]` i.e: `grep Port /etc/ssh/sshd_config`

### Search command history

Will search for the last command entry of the given input.
Press `ctrl + r` => enters the search mode, then type the command
Keep pressing `ctrl + r` to display the previous matching command

### Count the number of files in directory

`ls <fileName> | wc -l`

## Permissions

[Link](https://www.pluralsight.com/blog/it-ops/linux-file-permissions)

- [Change owner](#change-owner)
- [Change permission](#change-permission)

### Change owner

Command: **chown**

`sudo chown [user][:group] [fileName/folderName]`  
`sudo chown matt index.js` => will change the owner of the file _index.js_ to the user **matt**  
`sudo chown matt:admins index.js` => will change the owner of the file _index.js_ to the user **matt** and the group **admins**  
`sudo chown -R matt src` => will change the owner of the folder _src_ and everything in it to the user **matt**  

### Change permission

The 3 permissions are **read**, **write** and **execute**:
0 = No permission  
1 = Execute
2 = Write
4 = Read
Command: **chmod**: `sudo chmod [numericCode] [fileName/folderName]`  
`sudo chmod 777 index.js` => will give read, write and execute access to all users (**be very careful with this one**)  
`sudo chmod 777 index.js` => will give read, write and execute access to all users to the _index.js_ file (**/!\ be very careful with this one !**)  
`sudo chmod 700 src/` => will give read, write and execute access to the owner(user) of the folder _src/_ only

## Bash aliases

Create a _bash_aliases_ file to command shortcuts:  
`touch ~/.bash_aliases` => to create the file in the user's home directory  
`sudo nano ~/.bash_aliases` => to update the file  

```bash
alias <alias_name>="<commandto run>"
e.g: 
alias gis="git status" => will run the "git status" command
alias godev="cd ~/dev" => will navigate to the "dev" directory
```

Restart the OS or run the following command to enable the commands:  
`source ~/.bash_aliases`

## Kill WSL2

List the distros and their state:  
`wsl -l -v`  

Terminate everything:

`wsl.exe --shutdown`  
`wsl --shutdown`
`sudo killall -r '.*'`  

Terminate specific disto:  
`wsl -t <DistroName>`  

## Restart WSL 2

`wsl`
`wsl -d <DistroName>`

## Kill node process

List all node processes:  
`ps aux | grep node` || `ps -ef | grep node`

Display a process on a specific port:  
`lsof -i :<portNumber>` e.g. `lsof -i :3001`

Kill the process:  
`kill -9 <process_id>`
