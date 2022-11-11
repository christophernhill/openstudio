On a machine with Docker (e.g. your laptop)

* download image and save to tar file

    ```
    $ docker pull nrel/openstudio:develop
    $ docker image ls
    REPOSITORY                              TAG                   IMAGE ID       CREATED         SIZE
    nrel/openstudio                         develop               004d9b0959e0   9 hours ago     3.45GB
      :
      :
    $ docker save 004d9b0959e0 -o ~/projects/openstudio-docker.tar
    ```
    
* copy file to cluster

    ```
    $ scp openstudio-docker.tar cnh@eofe8.mit.edu:/nfs/cnhlab001/cnh/projects/openstudio/
    ```
    
    
On cluster with Singularity, for example, https://engaging-ood.mit.edu (singularity available by default) or https://txe1-portal.mit.edu/ (singularity conditionally available via approved request). 

* build singularity image

    ```
    ssh -l cnh eofe8.mit.edu
    cd /nfs/cnhlab001/cnh/projects/openstudio/
    module load singularity
    singularity build --sandbox openstudio.sdir  docker-archive://openstudio-docker.tar
    singularity shell openstudio.sdir
    Singularity> dpkg-deb -xv /OpenStudio-3.5.0+7b14ce1588-Ubuntu-20.04.deb /tmp/
    ```
  
* run openstudio in Singularity session

    ```
    $ singularity shell openstudio.sdir
    Singularity> /usr/local/openstudio-3.5.0/bin/openstudio 
    Usage: openstudio [options] <command> [<args>]

    -h, --help                       Print this help.
        --verbose                    Print the full log to STDOUT
    -I, --include DIR                Add additional directory to add to front of Ruby $LOAD_PATH (may be used more than once)
    -e, --execute CMD                Execute one line of script (may be used more than once). Returns after executing commands.
        --gem_path DIR               Add additional directory to add to front of GEM_PATH environment variable (may be used more than once)
        --gem_home DIR               Set GEM_HOME environment variable
        --bundle GEMFILE             Use bundler for GEMFILE
        --bundle_path BUNDLE_PATH    Use bundler installed gems in BUNDLE_PATH
        --bundle_without WITHOUT_GROUPS
                                     Space separated list of groups for bundler to exclude in WITHOUT_GROUPS.  Surround multiple groups with quotes like "test development".

    ```
