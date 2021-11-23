# Linux

- Basic commands
- bash_aliases
- kill WSL2

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

`wsl.exe --shutdown` or `sudo killall -r '.*'`
