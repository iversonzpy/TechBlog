# Commands for Productivity Improvement

## Customized commands

### 1. Command: rnfile - renaming a file replacing spaces
Want to define your own command?

For example, renaming your downloaded pdf paper name with its own title. But it contains lots of spaces and you want to replace those spaces with '_'.

Define a command, rnfile, for yourself.

**Step 1**, define a my_commands.sh at a location you want. For example, in your home directory(~)

```bash
vim my_commands.sh
```
In your my_commands.sh file, type those lines.
```
#!/bin/bash
function rnfile(){
      mv "$1" $(echo "$1" | tr ' ' '_');
      echo "Renaming done!"
}
```
**Step 2**, Save your file, give execution to this file
```
chmod +x ~/my_commands.sh
```
**Step 3**, Add the below compiling commands to your ~/.bashrc or ~/.zshrc file.
```
source ~/my_command.sh
```
Compile your ~/.bashrc or ~/.zshrc file.
```
source ~/.bashrc
```
**Test your command**, at any folder, you can rename a file replacing spaces with '_'. E.g.
```
touch a\ b.pdf
rnfile a\ b.pdf
```
File 'a b.pdf' will be renamed to 'a_b.pdf'.

### 2. How To SSH Into A Particular Directory On Linux ###

Ref: [How To SSH Into A Particular Directory On Linux](https://www.ostechnix.com/how-to-ssh-into-a-particular-directory-on-linux/)

on remoter server:

```
vim ~/.bash_profile
cd /path/to/folder >& /dev/null
source ~/.bash_profile
```

Or on local machine:

```
ssh -t ur-name@ur-remoter-server-ip 'cd /path/to/folder ; bash'
```