---
title: "Terminal Guide"
date: 2023-11-28
author: ["austin"]
publishDate: 2023-11-28T08:00:00Z
draft: false # change to true to have page render. Visit posts/austins_terminal_guide and it will render even if publish date has yet to come.
---

## Introduction

#### Welcome to the CLI

<font style="color:A0A0A0"> If you have never used the command line interface (CLI) before, then now is the time to embark on a journey to a new realm where simplicity meets power. It's the key to making computers do exactly what you want, how you want it, when you want it. It is a direct line of communication to your OS. As you learn each new command, you'll find yourself with the ability to conjure tasks and vanquish errors.</font>

<font style="color:A0A0A0">Do not be daunted by the stark text and blinking cursor. They are your allies on this journey. This is the beginning of a journey to technological enlightenment!
</font>

#### Terminal Access Across Different Operating Systems

<font style="color:A0A0A0">Accessing the Terminal can vary depending on the operating system in use. Here’s how to access it on the most commonly used systems:</font>

##### macOS
  - <font style="color:A0A0A0">Navigate to `/Applications/Utilities/` and double-click on Terminal.</font>
  - <font style="color:A0A0A0">Alternatively, use Spotlight by pressing `Command + Space`, typing "Terminal," and pressing `Enter`.</font>

##### Linux
  - <font style="color:A0A0A0">Use the keyboard shortcut `Ctrl + Alt + T` in most distributions.</font>
  - <font style="color:A0A0A0">For those using a graphical interface like GNOME, KDE, or XFCE, look for the Terminal in your applications menu.</font>

##### Windows
  - <font style="color:A0A0A0">For Windows 10 and later, access the Command Prompt or PowerShell via the Start menu or by typing "cmd" or "PowerShell" in the search bar. PowerShell follows the standard outlined in this guide more closely.</font>
  - <font style="color:A0A0A0">For advanced users, Windows Subsystem for Linux (WSL) can be installed to provide a rich Linux-like Terminal environment.</font>

<font style="color:A0A0A0">Once the Terminal window is open, you're presented with a prompt, followed by a blinking cursor. The initial prompt often contains information about the current user, the host system, and the present working directory.</font>

#### Warning

<font style="color:red">The Terminal is a powerful tool. Proceed with caution. Don't copy and paste random commands without understanding them unless you want to mess up your entire system.</font>

## Terminal Essentials


### Fundamental Commands
<font style="color:A0A0A0">Knowing a few fundamental commands is essential for navigating and operating within the CLI environment effectively. The commands listed here are foundational for file management, navigation, and system interaction. Master these, and you'll have taken your first step into a broader world of command-line proficiency.</font>
##### pwd
<font style="color:grey">Prints the current working directory path.</font>
```sh
pwd
```
##### cd
<font style="color:grey">Changes the current directory to the one specified. Without arguments, it changes to the home directory.</font>
```sh
cd <directory> # Example: `cd /home/user/documents`
```
##### ls
<font style="color:grey">Lists the contents of the current directory. Flags can be used to alter the output, such as `-l` for long listing or `-a` to show all files (including hidden ones).</font>
```sh
ls [options] [directory] # Example: `ls -la`
```
##### mkdir
<font style="color:grey">Creates a new directory with the specified name.</font>
```sh
mkdir <directory> # Example: `mkdir new_folder`
```
##### rm
<font style="color:grey">Removes files or directories. Use `-r` for recursive deletion (necessary for directories) and `-f` to force deletion without prompt.</font>
```sh
rm [options] <file/directory> # Example: `rm -rf old_folder`
```
##### rmdir
<font style="color:grey">Removes empty directories.</font>
```sh
rmdir <directory> # Example: `rmdir empty_folder`
```
##### mv
<font style="color:grey">Moves a file or directory, or renames it if the destination is within the same directory.</font>
```sh
mv <source> <destination> # Example: `mv old_name.txt new_name.txt`
```
##### cp
<font style="color:grey">Copies files or directories. Use `-r` to copy directories.</font>
```sh
cp [options] <source> <destination> # Example: `cp -r source_folder destination_folder`
```
##### touch
<font color="grey">Creates a new empty file or updates the timestamp on existing files.</font>
```sh
touch <file> # Example: `touch new_file.txt
```
#### <font style="color:black">Viewing and Editing Files</font>

<font style="color:A0A0A0">When working with files, you often need to view their contents or edit them. This section covers the basic commands and programs for viewing and editing files directly from the terminal.</font>

##### cat
<font style="color:grey">Concatenates and displays file contents. Commonly used to quickly view small files.</font>
```sh
cat <file> # Example: `cat example.txt`
```
##### more
<font style="color:grey">Views the contents of a file one page at a time. Move forward in the file with the spacebar and quit with `q`.</font>
```sh
more <file> # Example: `more example.txt`
```

##### less
<font style="color:grey">An improved version of `more`. It allows backward movement in the file as well as forward movement.</font>
```sh
less <file> # Example: `less example.txt`
```

##### nano
<font style="color:grey">A simple, easy-to-use text editor that runs in the terminal. Use `Ctrl + O` to save and `Ctrl + X` to exit.</font>
```sh
nano <file> # Example: `nano example.txt`
```

##### vi
<font style="color:grey">A classic text editor with a modal interface. Requires learning different modes but is very powerful.</font>
```sh
vi <file> # Example: `vi example.txt`
```
#### vim
<font style="color:grey">An improved version of `vi`. Includes syntax highlighting, a comprehensive help system, and a wide range of plugins.</font>
```sh
vim <file> # Example: `vim example.txt`
```

### <font style="color:black">Essential CLI Keyboard Shortcuts</font>

<font style="color:A0A0A0">Navigating the command line efficiently requires familiarity with keyboard shortcuts. These shortcuts enhance productivity and streamline your workflow. Depending on the system you are using, Mac/Windows/Linux, the equivalent keys may vary between Crl and Super, or something completely different. Regardless, these options exist but may be bound to another set of keys. Below are fundamental keyboard shortcuts for command-line use:</font>

#### Navigation
- `Ctrl + A` or `Home`: <font style="color:grey">Move the cursor to the beginning of the line.</font>
- `Ctrl + E` or `End`: <font style="color:grey">Move the cursor to the end of the line.</font>
- `Ctrl + B` or `Left Arrow`: <font style="color:grey">Move the cursor back one character.</font>
- `Ctrl + F` or `Right Arrow`: <font style="color:grey">Move the cursor forward one character.</font>
- `Alt + B`: <font style="color:grey">Move the cursor back one word.</font>
- `Alt + F`: <font style="color:grey">Move the cursor forward one word.</font>

#### Editing
- `Ctrl + U`: <font style="color:grey">Cut the text from the cursor to the beginning of the line.</font>
- `Ctrl + K`: <font style="color:grey">Cut the text from the cursor to the end of the line.</font>
- `Ctrl + W`: <font style="color:grey">Cut the word before the cursor.</font>
- `Alt + D`: <font style="color:grey">Cut the word before the cursor.</font>
- `Ctrl + Y`: <font style="color:grey">Paste the last cut text.</font>
- `Ctrl + _`: <font style="color:grey">Undo the last text modification.</font>

#### History
- `Ctrl + P` or `Up Arrow`: <font style="color:grey">Go to the previous command in the history.</font>
- `Ctrl + N` or `Down Arrow`: <font style="color:grey">Go to the next command in the history.</font>
- `Ctrl + R`: <font style="color:grey">Reverse search command history.</font>
- `Ctrl + G`: <font style="color:grey">Exit history searching mode without running a command.</font>
- `Ctrl + S`: <font style="color:grey">Forward search command history (note that this may be disabled in some terminal configurations. Sometimes you can change this terminal setting with `stty -ixon`).</font>

#### Control
- `Ctrl + C`: <font style="color:grey">Terminate the current process.</font>
- `Ctrl + Z`: <font style="color:grey">Suspend the current process by sending it to the background.</font>
- `Ctrl + D`: <font style="color:grey">Exit the current shell. When used at the prompt, it logs out of the session, similar to running the `exit` command.</font>

#### Miscellaneous
- `Ctrl + L`: <font style="color:grey">Clear the screen (similar to running the `clear` command).</font>
- `Ctrl + T`: <font style="color:grey">Swap the last two characters before the cursor.</font>
- `Alt + T`: <font style="color:grey">Swap the last two words before the cursor.</font>
- `Ctrl + X`, followed by `Ctrl + E`: <font style="color:grey">Open the current line in the default command-line editor (as set in the `EDITOR` bash variable) for a full-screen editing experience. </font>
- `Alt + <number>`, followed by key press: <font style="color:grey">Press some key a given number times. For example: </font> `Alt+12+delete` <font style="color:grey">delete 12 characters </font>

<font style="color:A0A0A0">These shortcuts are based on the GNU Readline library, which is utilized in various command-line interfaces, including many shells. Specific functionality may vary depending on your shell configuration and terminal emulator.</font>


### <font style="color:black">Output</font>

#### <font style="color:A0A0A0">Messaging Commands</font>
<font style="color:grey">These commands allow for displaying and sending messages in the terminal.</font>
##### echo
<font color="grey">Displays a line of text/string that is passed as an argument.</font>
```sh
echo <string> # Example: `echo "Hello, World!"`
```
##### printf
<font color="grey">Similar to `printf` in C. Usually installed by default on most systems and allows to tweak the output a little finer.</font>
```sh
printf <string> [$var] [$var] ... # Example: printf "%s has logged in %d times\n" $USER $TIMES
```
##### wall
<font color="grey">Broadcasts a message to all users logged into the system.</font>
```sh
wall <string> # Example: `wall "The server will restart in 10 minutes!"`
```

### <font style="color:black">Advanced Command Execution</font>

#### <font style="color:A0A0A0">Output Redirection</font>
<font style="color:grey">Directing the flow of data to and from files or programs.</font>
- `>`: Redirects output to a file, overwriting previous content.
- `>>`: Redirects output to a file, appending to any existing content.
- `|`: Sends the output from one command to another command as input.
- `<`: Redirects input from a file to a command.

### <font style="color:black">Streamlining Text Processing</font>

#### <font style="color:A0A0A0">Tools</font>
<font style="color:grey">Powerful command-line utilities for text processing.</font>
##### grep
<font color="grey">Searches through text using patterns.</font>
```sh
grep [options] <pattern> [file...] # Example: `grep "search_term" example.txt`
```
##### sed
<font color="grey">Stream editor for filtering and transforming text.</font>
```sh
sed [options] [script] [input_file...] # Example: `sed 's/old/new/' example.txt`
```
##### awk
<font color="grey">A programming language for text processing and data extraction.</font>
```sh
awk [options] [program] [file...] # Example: `awk '/search_pattern/ { action_to_take_on_matches; another_action; }' example.txt`
```


### <font style="color:black">File Ownership and Permissions</font>

#### <font style="color:A0A0A0">Changing Ownership</font>
<font color="grey">Commands to change the owner and group of files and directories.</font>
##### chown
<font color="grey">Change the owner and/or group of each given file or directory.</font>
```sh
chown <owner>[:<group>] <file> # Example: `chown alice:alice file.txt`
```

#### <font style="color:A0A0A0">Modifying Permissions</font>
<font color="grey">Adjust the read, write, and execute permissions on files and directories.</font>
##### chmod
<font color="grey">Change the file mode bits for file access permissions.</font>
```sh
chmod <mode> <file> # Example: `chmod 755 script.sh`
```
##### chgrp
<font color="grey">Change the group ownership of files or directories.</font>
```sh
chgrp <group> <file> # Example: `chgrp developers script.sh`
```

### <font style="color:black">System Process Management</font>

#### <font style="color:A0A0A0">Monitoring</font>
<font color="grey">Keeping track of system processes and their states.</font>
##### ps
<font color="grey">Reports a snapshot of the current processes.</font>
```sh
ps aux # Show all running processes
```
##### top
<font color="grey">Displays Linux tasks and provides a dynamic real-time view of a running system.</font>
```sh
top # Start the top program
```
##### htop
<font color="grey">Interactive process viewer, considered an improved version of `top`.</font>
```sh
htop # Interactive process viewer (may require separate installation)
```

#### <font style="color:A0A0A0">Termination</font>
<font color="grey">Commands to stop running processes.</font>
##### kill
<font color="grey">Sends a signal to a process, typically to stop the process.</font>
```sh
kill <PID> # Example: `kill 12345`
```
##### pkill
<font color="grey">Allows sending signals to processes by pattern.</font>
```sh
pkill <pattern> # Example: `pkill firefox`
```

### <font style="color:black">System and Performance Insights</font>

#### <font style="color:A0A0A0">System Info</font>
<font color="grey">Commands to display user and system information.</font>
##### who
<font color="grey">Shows who is logged on to the system.</font>
```sh
who # List logged-in users
```
##### w
<font color="grey">Displays who is logged in and what they are doing.</font>
```sh
w # Detailed listing of logged-in users
```
##### neofetch <font style="color:989898">(not installed by default)</font>
<font color="grey">A command-line system information tool that displays system information alongside an image, generally

 your OS logo.</font>
```sh
neofetch # Show system information with ASCII art of the OS logo (requires installation)
```


## Information
<font style="color:A0A0A0">If you don't know what something does, you can use one of several documentation tools to check out what it does.</font>


##### man
<font style="color:grey">man (manual) gives you the user man-page for a command. A goto for nearly all commands.</font>
```sh
man <command> # Example: `man find`
# Or for multiple page entries
man [page number] <command> # Example: `man 1 man`
```


##### info
<font style="color:grey">info gives you the newer user manual for a command. Info is more comprehensive. It has basic hyperlinking and markup and vim navigation bindings. Some describe info-pages it as a guide instead of a manual. Not everything has an info page.</font>
```sh
info <command> # Example: `info info`
```
<font style="color:grey">You can run info on its own to get a list of all available commands with an info page.</font>
```sh
info
```


#### help
<font style="color:grey">Summary of the usage of a command.</font>
```sh
<command> --help # Example: man --help
```

##### whatis
<font style="color:grey">Display one-line manual page descriptions.</font>
```sh
whatis <command> # Example: whatis whatis
```

##### type
<font style="color:grey">  Display the type of command the shell will execute.</font>
```sh
type <command> # Example: type type
```
##### which
<font color="grey">Locates the executable file associated with the given command.</font>
```sh
which <command> # Example: `which ls`
```

##### tldr <font style="color:989898">(not installed by default)</font>
<font style="color:grey"> This stands for "Too long: didn't read". This provides a super short version of the info, man, and help pages. This has to be installed separately on most systems.</font> 
```sh
tldr <command> # Example: tldr tldr
```

```sh
tldr tldr # Example tun of tldr on tldr.
```
```
tldr

  Display simple help pages for command-line tools from the tldr-pages project.
  More information: https://tldr.sh.

  - Print the tldr page for a specific command (hint: this is how you got here!):
    tldr command

  - Print the tldr page for a specific subcommand:
    tldr command-subcommand

  - Print the tldr page for a command for a specific [p]latform:
    tldr -p android|linux|osx|sunos|windows command

  - [u]pdate the local cache of tldr pages:
    tldr -u
```

#### cheat <font style="color:989898">(not installed by default)</font>
<font style="color:grey">Provides a cheat sheet for using the command</font>
```sh
cheat <command> # Example: cheat cheat
```

```sh
cheat cheat # Example run of cheat on cheat.
```
```
# To see example usage of a program:
cheat <command>

# To edit a cheatsheet
cheat -e <command>

# To list available cheatsheets
cheat -l

# To search available cheatsheets
cheat -s <command>

# To get the current `cheat' version
cheat -v
```
