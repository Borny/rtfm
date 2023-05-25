# WSL - Windows Subsystem for Linux

- [Basic commands](#basic-commands)

## Basic commands

[Windows commands link](https://learn.microsoft.com/en-us/windows/wsl/basic-commands)

This command must be ran from a **command prompt** or **PowerShell** in Windows.

- List available distributions: `wsl --list --online` or `wsl -l -o`
- List installed distributions: `wsl --list --verbose` or `wsl -l -v`
- Terminate all running distributions and WSL: `wsl --shutdown`
- Terminate a specific distribution: `wsl --terminate <Distribution Name>`
- Uninstall a specific distribution: `wsl --unregister <DistributionName>`
- Set the WSL version that a distribution uses: `wsl --set-version <distribution name> <versionNumber>`
- Set the default WSL version that a new distribution will use: `wsl --set-default-version <version>`
- Set the default Linux distribution: `wsl --set-default <Distribution Name>`
- Run a specific Linux distribution: `wsl --distribution <Distribution Name> --user <User Name>`
- Update WSL: `wsl --update`
- Display WSL information: `wsl --status`
- WSL version: `wsl --version`
- Help command: `wsl --help`
- Run as a specific user: `wsl -u <Username>` or `wsl --user <Username>`
- Set the default user for a distribution: `<DistributionName> config --default-user <Username>`
- Mount a disk or device: `wsl --mount <DiskPath>`
- Unmount a disk or device: `wsl --unmount <DiskPath>`
