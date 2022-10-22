# resync
Resync is a bash script that automates the process of resyncing sources for android ROMS.  
In addition to this, it can also read from mini-files to automatically apply patches to a rom's source.  

## Process
Resync relies on a few main things:  
The script itself, resync.dirs, resync.post, and files ending in .mod to apply source patches  
  
Resync runs `repo sync` once and captures its output in order to get the conflicting directories that need to be deleted (which, when there are directories with manual modifications, repo sync does not fully update the rom's source)  
This output is piped to `resync.dirs` and trimmed so that it becomes a list of directories that are then deleted.  
`repo sync` is then ran again, after which `resync.post` is sourced to clone what may have deleted due to conflicting with repo (e.g. hals, source optimizations, etc.) that are not a part of the rom's source itself and thus would need to be recloned manually  
Following this, resync will source all files ending in `.mod` that you may make in order to automatically patch parts of the rom's source, via sed/awk commands or other things.  
