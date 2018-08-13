# HUDL
A bash utlity to
- Execute commands in a cluster of machines
- Execute commands in the background
- Copy files/folders to all machines

## Options
`-h ARGUMENT` : Takes hosts file with the following format
A host file would have list of ip_address or ip_address with slots keyword
such as 
```
172.16.12.12 slots=8
172.16.12.13
```

`-t` : Runs command in background in a detached tmux session so you can execute in parallel over many machines
The tmux session automatically ends after the command exits

`-l` : Only used along with -t option. Logs the output of that tmux session 
in a file with name `run_20180811_005726Z` 
where the second part is the timestamp of when the command was run

`-v` : Verbose mode, prints the IP of each node on which a command will be executed

`-c ARGUMENT` : Source path of file/folder to be copied

`-d ARGUMENT` : Destination path of file/folder to be copied. 
Defaults to empty (i.e. home folder of destination host)

## Installation

Here's a one liner to get you started

```
sudo sh -c "curl https://gist.githubusercontent.com/rahul003/6922d91743ee96eefbeb1025ac1d1141/raw/899ff6156de3dba0c47c7b4fd91e6639610759d7/run_clust.sh -o /usr/local/bin/hudl && chmod +x /usr/local/bin/hudl"
```
## Examples

* Copying file to a folder `hudl -v -h hosts -c efs/data/caltech-256/256_ObjectCategories.tar -d data/`
* Running a command in the background and logging the execution of the command `hudl -h hosts -v -t -l pip3 install tensorflow`
* Running a command silently in the background `hudl -h hosts -t pip3 install tensorflow`

## Why the name Hudl?
Hudl is a reference to a Huddle by a sports team where the coach instructs the team what to do, in this case for all nodes to perform a task
