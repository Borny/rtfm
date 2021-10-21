# Windows

- Format USB flash drive

## Format USB flash drive

- Open the command prompt
- `diskpart` => will open a new terminal
- `list disk` => will list all the available disks/drives on the system
- `select disk [diskNumber]` => will select the required disk
- `list disk` => the required disk should now have an hyphen before it
- `clean` => will erase all data on the disk
- `create partition primary`
- `select partition 1`
- `format fs=[ntfs | fat] quick` => will format the drive to the desired format. **ntfs** is the most used format on Windows
- `active`
- [optional] => `assign letter=[letterToAssign]` => will assign a letter to the drive. i.e: Y,X,W.... 
 
