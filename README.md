# resync
Resync is a bash script that automates the process of resyncing sources for android ROMS.  
In addition to this, it can also read from mini-files to automatically apply patches to a rom's source.  

## Process
Resync relies on a few main things:  
The script itself, `resync.dirs`, `resync.post`, and files ending in `.mod` to apply source patches  
  
Resync runs `repo sync` once and captures its output in order to get the conflicting directories that need to be deleted (which, when there are directories with manual modifications, repo sync does not fully update the rom's source)  
This output is piped to `resync.dirs` and trimmed so that it becomes a list of directories that are then deleted.  
`repo sync` is then ran again, after which `resync.post` is sourced to clone what may have deleted due to conflicting with repo (e.g. hals, source optimizations, etc.) that are not a part of the rom's source itself and thus would need to be recloned manually  
Following this, resync will source all files ending in `.mod` that you may make in order to automatically patch parts of the rom's source, via sed/awk commands or other things.  


## Configuration
`ROOT`, `DIRS`, and `CMDS` are declared at the beginning of the script and may be changed to whatever you like  
  
`ROOT` is the directory that should store DIRS, CMDS, and whatever `.mod` files you create.  
`DIRS` (`resync.dirs`) is where the output of the "first run" of `repo sync` is piped, and shouldn't need to be touched.  
`CMDS` (`resync.post`) is where you should add the commands you want to be run after the second repo sync.  
`.mod` files are sourced as bash scripts and can be whatever you want them to be. An example of a use would be to run a few `sed` or `awk` commands to modify parts of a rom's source.  

Keep in mind that `CMDS` and `.mod`s are optional. If you do not have a use for them, you may leave them empty. You can also put the source patches inside `CMDS` if you want source patches and post-`repo sync` actions to be sourced from the same file.  
  
If `ROOT`, `DIRS`, or `CMDS` do not exist upon running the script, they will be automatically created.  
