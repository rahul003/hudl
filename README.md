# HUDL
A simple bash utlity to help ease the task of managing a cluster of machines.
It supports
- Executing commands on a cluster of machines
- Executing commands in the background
- Copying files/folders to all machines
- Executing a script on a cluster of machines (copy + execute)

## Options
`-h ARGUMENT` : Takes hosts file with the following format
A host file would have list of ip_address. An address can also have the suffix `slots=x` as would be used for MPI. Hudl snips out the slots variable for the line to extract the IP address/host name.
```
172.16.12.12 slots=8
172.16.12.13
```

`-t` : Runs command in background in a detached tmux session so you can execute in parallel over many machines
The tmux session automatically ends after the command exits

`-l` : Only used along with -t option. Logs the output of that tmux session 
in a file with name `run_20180811_005726Z` in the folder `~/hudl/tmux_logs/`
where the second part is the timestamp of when the command was run

`-c ARGUMENT` : Source path of file/folder to be copied

`-d ARGUMENT` : Destination path of file/folder to be copied. 
Defaults to empty (i.e. home folder of destination host) when copying files. 
Defaults to `~/hudl/scripts/` when running in script mode (`-s` option below).

`-s ARGUMENT` : Script path 
It copies the script to destination path and runs it with `bash {script_name.sh}`, i.e. bash followed by the script name. 
You can control destination path where script will be coped using `-d` argument, else it defaults to `~/hudl/scripts/`. 
The command can also be controlled such as to specify arguments, just pass the command after all named options.

`-v` : Verbose mode, prints the IP of each node on which a command will be executed along with the commands to be executed

## Installation

Here's a one liner to get you started

```
sudo sh -c "curl https://gist.githubusercontent.com/rahul003/6922d91743ee96eefbeb1025ac1d1141/raw/899ff6156de3dba0c47c7b4fd91e6639610759d7/run_clust.sh -o /usr/local/bin/hudl && chmod +x /usr/local/bin/hudl"
```
## Examples

* hudl -h test-hosts -v touch test
* hudl -h test-hosts -v -c hello/ -d rahul/
* hudl -v -h test-hosts -c efs/data/caltech-256/256_ObjectCategories.tar
* hudl -h test-hosts -v -t pip3 install tensorflow
* hudl -h test-hosts -v -t -l pip3 install tensorflow

## Why Hudl?
Hudl is a reference to Huddles by a sports team where the coach instructs the team what to do, in this case for all nodes to perform a task