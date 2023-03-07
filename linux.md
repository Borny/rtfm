# Linux

- [Basic commands](#basic-commands)
- [bash_aliases](#bash_aliases)
- [kill WSL2](#kill-wsl2)
- [Kill node process](#kill-node-process)

## Basic commands

- ls
- cd
- grep
- ctrl + r

### ls

**list system** => will list all the files and folders in the required directory

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

### ctrl + r

Will search for the last command entry of the given input.

## bash_aliases

## Kill WSL2

List the distros and their state:  
`wsl -l -v`  

Terminate everything:

`wsl.exe --shutdown`  
`wsl --shutdown`
`sudo killall -r '.*'`  

Terminate specific disto:  
`wsl -t <DistroName>`  

## Restart WSL

`wsl`
`wsl -d <DistroName>`

## Kill node process

List all node processes:  
`ps aux | grep node` || `ps -ef | grep node`

Display a process on a specific port:  
`lsof -i :<portNumber>` e.g. `lsof -i :3001`

Kill the process:  
`kill -9 <process_id>`
