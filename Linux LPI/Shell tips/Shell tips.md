## Keyboard shortcut and commands
 
 `\` (backslash)

The backslash escape character can be used before a shell command to override any aliases.
For example if _rm_ was made into an alias for _rm -i_ then typing “rm” would actually run _rm -i_.
However, typing _\rm_ lets the shell ignore the alias and just run _rm_ (its runs exactly what you type), this way it won't confirm if you want to delete things.

 `script`

The “_script_” command creates a typescript, or "capture log" of a shell session - it writes a copy of your session to a file, including commands you type and their output.

`~` (tilde character)

- The tilde character is used as an alias to a users home directory.
	For example, if your user-name was “fred”, instead of typing _cd /home/fred_ you could simply type _cd ~._ Or to get to fred's tmp directory (under his home directory) you could type _cd ~/tmp._
 - It can also be used as a shortcut to other users home directories, simply type: ~user_name and it will take you to the users home directory. Note that you need to spell the username exactly correct, no wildcards.
`CTRL R` Searching through the Command History
`CTRL d` Log out from the terminal
`CTRL Z` puts process in background --- **fg** to bring it back 
`CTRL -A`  and  `CTRL -E`
- These key combinations are used for going to the start and end of the line on the command line. Use **CTRL**-**A** to jump to the start of the line, and **CTRL**-**E** to jump to the end of the line.
`CTRL -K`
- This key combination can be used to cut or delete what is currently in front of the cursor.
`CTRL -W`
- This key combination can be used to cut or delete the entire line that has being typed.

### Manuals
`info` 
- Provides a more detailed hyper-text manual on a particular command, this only works for some commands.
`whatis`
- Displays a one-line description of what a program does. The string needs to be an exact match, otherwise _whatis_ won't output anything. Relies on the whatis database (see below).

## Directing Input/output

### Concept Definitions

All three of the following definitions are called “ File Streams.” They hold information that is either received from somewhere or sent to somewhere. In a UNIX system, the keyboard input (standard input), information printed to the screen (standard output) and error output (also printed to the screen) are treated as separate File Streams.

#### Standard output

Standard output is the output from the program printed to the screen, not including error output (see below).

#### Standard input

Standard input is the input from the user. Normally the keyboard is used as the standard input device in a UNIX system.

#### Standard error

Standard error is error output from programs. This output is also sent to the screen and will normally be seen mixed in with standard output. The difference between standard output and standard error is that standard error is unbuffered (it appears immediately on the screen) and standard error is only printed when something goes wrong (it will give you details of what went wrong).

### Usage
`>`
- The greater than symbol is used to send information somewhere (for example a text file)
- Example: `cat file1 file2 > file1_and_2.txt`
- This will concatenate the files together into one big file named “file1_and_2.txt”. Note that this will overwrite any existing file.

`<`
- The less than symbol will insert information from somewhere (a text file) as if you typed it yourself. Often used with commands that are designed to get information from standard input only.
- For example (using tr): `tr '[A-Z]' '[a-z]' < fileName.txt > fileNameNew.txt`
- The example above would insert the contents of “fileName.txt” into the input of _tr_ and output the results to “fileNameNew.txt”.

`>>`
- The >> symbol appends (adds) information to the end of a file or creates one if the file doesn't exist.

`<<`

- The << symbol is sometimes used with commands that use standard input to take information. You simply type _<< word_ (where word can be any string) at the end of the command. However its main use is in shell scripting.
- The command takes your input until you type “word”, which causes the command to terminate and process the input.
- Using << is similar to using **CTRL**-**D** (EOF key)_,_ except it uses a string to perform the end-of-file function. This design allows it to be used in shell scripts.
- For example type "cat" (with no options...) and it will work on standard input.
- To stop entering standard input you would normally hit **CTRL**-**D** .
- As an alternative you can type "cat << FINISHED", then type what you want.
- When you are finished, instead of hitting **CTRL**-**D** you could type "FINISHED" and it will end (the word FINISHED will not be recorded).

`2>`

- Redirects error output. For example, to redirect the error output to /dev/null, so you do not see it, simply append this to the end of another command...
- For example: `make some_file 2> /dev/null`
- This will run make on a file and send all error output to /dev/null

`|`

- The “pipe” command allows the output of one command to be sent to the input of another.
- For example: `cat file1.txt file2.txt | less`
- Concatenates the files together, then runs _less_ on them. If you are only going to look at a single file, you would simply use _less_ on the file...

`tee`

- Sends output of a program to a file and to standard output. Think of it as a T intersection...it goes two ways.
- For example: ls /home/user | tee my_directories.txt
- Lists the files (displays the output on the screen) and sends the output to a file: “my_directories.txt”.

`&>`
- Redirects standard output and error output to a specific location.
- For example: `make &> /dev/null`
- Sends both error output and standard output to /dev/null so you won't see anything.
### Command Substitution

Command substitution is basically another way to do a pipe, you can use pipes and command substitution interchangeably, it's up to you which one you find easier...

Command substitution can be done in two distinct ways.

1. Method One (back-quotes)

Simply type: 
```sh 
$ > command_1 `command_2 -options`
```

This will execute “command_2” and it's output will become the input to “command_1”.

2. Method Two (dollars sign)

Simply type:
`command_1 $(command_2)`

This will execute “command_2” and it's output will become the input to “command_1”.

3. Using the pipe instead

You can of course use pipes to do the same thing, if you don't know what a pipe is, please see above 
For example instead of doing: `less $cat file1.txt file2.txt`

You could do: `cat file1.txt file2.txt | less`

And end up with exactly the same result, it's up to you which way you find easier.

### Performing more than one command

1. Executing the second command only if the first is successful
- To do this you would type: `command1 && command2`
2. Executing the second command only if the first fails
- `command1 || command2`
3. Executing commands sequentially
- `command1; command2`

## working with the file system

`find`

_find_ is a tool which looks for files on a filesystem. _find_ has a large number of options which can be used to customise the search.

Basic example: `find / -name file

This would look for a file named “file” and start at the root directory (it will search all directories including those that are mounted filesystems).

The _-name_ option is case sensitive you can use the _-iname_ option to find something regardless of case.

Use the _-regex_ and _-iregex_ to find something according to a regular expression (either case sensitive or case insensitive respectively).

The `-exec` option is one of the more advanced find operations. It executes a command on the files it finds (such as moving or removing it or anything else...).

`slocate`

_slocate_ outputs a list of all files on the system that match the pattern, giving their full path name (it doesn't have to be an exact match, anything which contains the word is shown).

`whereis`

whereis locates the binary, source, and manual page for a particular program, it uses exact matches only, if you only know part of the name use _slocate_.

Command syntax: `whereis program_name`

`which`

Virtually the same as whereis, except it only finds the executable (the physical program). It only looks in the PATH (environment variable) of a users shell.

Use the _-a_ option to list all occurances of the particular program_name in your path (so if theres more than one you can see it).

Command syntax: `which program_name`

##  Working with files and folders

`shred`

Securely remove a file by overwriting it first. Prevents the data from being recovered by software (and even by most hardware), please be very careful when using shred as you may never be able to retrieve the data you have run the application on.

For example: `shred -n 2 -z -v /dev/hda1`

“What this tells shred, is to overwrite the partition 2 times with random data (- n 2) then finish it up by writing over it with zeroes (-z) and show you its progress (-v). Of course, change /dev/hda1 to the correct partition . Each pass can take some time, which is why I set it to only do 2 random passes instead of the default 25. You can adjust this number, of course, to your particular level of paranoia and the amount of time you have.

Since shred writes on such a low-level, it doesn't actually matter what kind of filesystem is on the partition--everything will be unrecoverable. Once shred is finished, you can shutdown the machine and sell or throw away the drive with peace of mind.

`du`
Displays information about file size. Use _du filename_ to display the size of a particular file. If you use it on directories it will display the information on the size of the files in the directory and each subdirectory.

Options for du (use _du -option(s)_):

- `-c` -- this will make _du_ print a grand total after all arguments have being processed.
    
- `-s` -- summarises for each argument (prints the total).
    
- `-h` -- prints things in “ human readable” mode; for example printing 1M (megabyte) rather than 1,024,000 (bytes).

`file`
Attempts to find out what type of file it is, for example it may say it's: binary, an image file (well it will say jpeg, bmp et cetera), ASCII text, C header file and many other kinds of files, it's a very useful utility.

Command syntax: `file file_name`

`stat`
Tells you detailed information about a file, including inode number creation/access date. Also has many advanced options and uses.

For simple use type: `stat file_name`

`split`
Splits files into several smaller files.

Use the _-b xx_ option to split into _xx_ bytes, also try _-k_ for kilobytes, and _-m_ for megabytes. You can use it to split text files and any other files... you can use _cat_ to re-combine the files.

This may be useful if you have to transfer something to floppy disks or you wish to divide text files into certain sizes.

Command syntax: `split -option file`









Using the _-hs_ options on a directory will display the total size of the directory and all subdirectories.