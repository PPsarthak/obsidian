#linux #placement-preparation #one-shot-notes 

**What is a terminal?**
a terminal is just a program that lets you issue text-based commands and renders the output of those commands

**What is the program that runs those commands???**
That's a shell.
#### Commands

| Command                                    | Description                                                                                                                    | Additional Info                                                                          |
| :----------------------------------------- | :----------------------------------------------------------------------------------------------------------------------------- | :--------------------------------------------------------------------------------------- |
| `pwd`                                      | print working directory                                                                                                        | the first / represents root directory                                                    |
| `ls -la`                                   | lists the files in current wd                                                                                                  | '-l' flag is used to list additional info and '-a' for hidden files                      |
| `cd`                                       | change directory                                                                                                               | '..' is used to move to parent directory <br>cd ~ is used to move back to home directory |
| `cat`                                      | view contents of file                                                                                                          |                                                                                          |
| `head -n (n) file.txt`                     | prints the first 'n' lines of a file                                                                                           | if nothing is provided, 10 is the default number                                         |
| `tail -n (n) file.txt`                     | prints the last 'n' files of a file                                                                                            |                                                                                          |
| `less -N file.txt`                         | let's you view the file                                                                                                        | '-N' flag displays the line numbers. Press 'q' to exit                                   |
| `touch (filenames)`                        | updates the modification time of specified file to current time <br> if file DNE, it creates the file                          |                                                                                          |
| `mkdir (name)`                             | creates a directory with specified name                                                                                        |                                                                                          |
| `mv (path1) (path2)`                       | moves a file or directory from one location to another <br> also allows to rename the file (add the new name instead of path2) |                                                                                          |
| `cp (path1) (path2)`                       | copies the file/directory from path1 to path2                                                                                  |                                                                                          |
| `rm (path)`                                | deletes a file or empty directory:                                                                                             | -r flag will remove all the subfolders "recursively"                                     |
| `grep "hello"` <br> `hello.txt hello2.txt` | searches the string hello in specified file(s)                                                                                 | `grep -r "hello" .` this will perform recursive search <br> of the string                |
| `find (path) -name (filename)`             | finds the file using it's name in the specified path                                                                           | `find some_directory -name "*.txt"` <br>`find some_directory -name "*chad*"`             |
| `whoami`                                   | prints the name of user                                                                                                        |                                                                                          |
| `chmod -R u=rwx,g=,o= DIRECTORY`           | recursively updates the permissions for (user) owner/groups/other                                                              | -r flag won't work; -R is used for recursion                                             |
| `sudo chown -R (ownerName) (path)`         | changes the owner to (owner) for the specified path                                                                            | sudo is required !!!                                                                     |
| `env`                                      | shows all the environment variables                                                                                            |                                                                                          |
| `export NAME="Lane"`                       | create a env variable                                                                                                          | `echo $NAME` prints the value of variable                                                |

#### File Permission Strings
It is a 10 digit long string 
The first char represents whether it is a file or a directory
	`-` = file
	`d` = directory
The next 9 chars are divided into groups of 3. Each representing read/write/execute permission
![[File Mode in OS.png|500]]

- The first 3 characters are "owner" permissions. The "owner" is usually just the user who created the file or directory, but it can be manually changed.
- The next 3 characters are "group" permissions. Unix-like systems support groups of users and a file or directory can be assigned to a single group. To be honest, unless you're a system administrator, you won't often worry about groups.
- The last 3 characters are "others" permissions. This is everyone else.
#### Vim
`vim (filename)` opens the file in vim editor
There are 3 modes in which the vim operates:
1. Command mode: when you start vim, you are in command mode
2. Insert Mode: when you want to make changes, press `I` to go in insert mode and make edits in the file. Press `ESC` twice to go back to command mode
3. View mode: read-only mode
##### View Mode commands
dd - delete current line
yy- yank/copy the current line
p - paste the copied line below cursor; P - paste above the cursor
## Cheat Sheet
![[Linux Cheat Sheet.png]]