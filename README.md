# HUDL
A simple bash utlity to help ease the task of managing a cluster of machines.
It supports
- Executing commands on a cluster of machines
- Executing commands in the background
- Copying files/folders to all machines using rsync
- Executing a script on a cluster of machines (copy + execute)

## Options
`-h ARGUMENT` : Takes hosts file with the following format
A host file would have list of ip_address. An address can also have the suffix `slots=x` as would be used for MPI. Hudl snips out the slots variable for the line to extract the IP address/host name.
```
172.16.12.12 slots=8
172.16.12.13
```

`-n ARGUMENT`: Number of hosts to read from host file. Picks the first `n` hosts

`-t` : Runs command in background in a detached tmux session so you can execute in parallel over many machines
The tmux session automatically ends after the command exits

`-l` : Only used along with -t option. Logs the output of that tmux session 
in a file with name `run_20180811_005726Z` in the folder `~/hudl/tmux_logs/`
where the second part is the timestamp of when the command was run

`-c ARGUMENT` : Source path of file/folder to be copied. Uses rsync for copying incrementally

`-d ARGUMENT` : Destination path of file/folder to be copied. 
Defaults to empty (i.e. home folder of destination host) when copying files. 
Defaults to `~/hudl/scripts/` when running in script mode (`-s` option below).

`-s ARGUMENT` : Script path 
It copies the script to destination path and runs it with `bash {script_name.sh}`, i.e. bash followed by the script name. 
You can control destination path where script will be coped using `-d` argument, else it defaults to `~/hudl/scripts/`. 
The command can also be controlled such as to specify arguments, just pass the command after all named options.

`-v` : Verbose mode, prints the IP of each node on which a command will be executed along with the commands to be executed

`-q`: Quiet mode for rsync. Suppress non error messages from rsync when using verbose mode. When not in verbose mode, this has no effect.

## Installation

Here's a one liner to install `hudl` into /usr/local/bin, so you can just execute `hudl` by typing on your terminal

```
sudo sh -c "curl https://raw.githubusercontent.com/rahul003/hudl/master/hudl -o /usr/local/bin/hudl && chmod +x /usr/local/bin/hudl"
```
## Examples
* Running a command on all machines sequentially and see the output on screen
```
hudl nvidia-smi
```
* Running a command on all machines in the background
```
hudl -t pip3 install tensorflow 
```
* Copying a folder to all machines and in verbose mode
```
hudl -v -c efs/data/caltech-256/256_ObjectCategories.tar -d data/
```
* Running a command in the background and logging the execution of the command 
```
hudl -vtl pip3 install mxnet
```
* Copying a local script to all machines and executing it
```
hudl -s setup.sh
```
* Copying a local script to all machines in a specific folder and executing it by passing some arguments (`arg1 arg2` in this case)
```
hudl -d installation-scripts/ -s setup.sh arg1 arg2
```
* Specifying hosts from a different file for the command
```
hudl -h server-hosts ps aux | grep python
```
* Running a sequence of commands by enclosing them within quotes, and choosing to use only the first `n` hosts from the hostfile
```
hudl -n 2 "start.sh && ps aux | grep python"
```
* Copying in the background while logging
```
hudl -tvlc source/ -d destination/
```

## Why the name Hudl?
Hudl is a reference to a Huddle by a sports team where the coach instructs the team what to do, in this case for all nodes to perform a specified task 
