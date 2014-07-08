=====================
HOWTO Reference Guide
=====================

A reference guide for things that may interest only me.

Transfer Large files
====================

To transfer a large file from one server to another without worrying about timeouts or dropped connections 
use rsync instead of scp:

    ::
        
        rsync -v --partial --progress --rsh=ssh local_file user@server:remote_file
        rsync -rvz --partial --progress --rsh='ssh -p 17235' local_file user@server:remote_file

        rsync -avxP --delete /path/to/directory/to/backup /path/to/directory/for/storing/backup

        -a means archive mode
        -v means print out the verbose information during backuping
        -x means not cross filesystem boundaries
        -P is shortcut for
            --progress means printing information showing the progress of the transfer
            and
            --partial means keeping partially transferred files if the transfer is interrupted
            
        rsync -aAXv /* /path/to/backup/folder --exclude={/dev/*,/proc/*,/sys/*,/tmp/*,/run/*,/mnt/*,/media/*,/lost+found}    
 

Reference: http://yyab.wordpress.com/2006/12/18/resume-a-large-scp-transfer/


Undo SVN Add
============

Adding new files and directories to SVN can sometimes include files and directories that should not be
included.  Remove them via the following command:

    ::
    
        svn rm --keep-local FILENAME
        
Relocate SVN Directory
======================

Relocating something in SVN is easy but I don't do it enough to remember the the easiest way to elimiate the .svn directories.
Here are different ways to do it.

    ::
    
        svn export –force RepositoryURL /path/to/target

        find . -type d -name .svn -exec rm -rf {} \;
        find . -type d -name .svn -exec rm -rf {} +
        
        
Bash Setting and DOT (.) Files
==============================

GitHub has some good information on setting up dot files.

http://dotfiles.github.io


Regular Expressions in vim
==========================

Search and replace in vim is a powerful feature.  You can use regular expressions like so
    ::

        :%s#\(.*\)=\(.*\) \(.*\)$#\1 \3#


Use UP ARROW to recall previous searches and commands.


Bash default commands:
======================

By default bash will use emacs as the command line editor.  Therefore it pays to
know a few emacs commands until your custom settings are installed.  Here is a decent 
place to start: http://www.hypexr.org/bash_tutorial.php


Kill Running Process:
=====================

When running things such as anisible scripts that may hang, this is how you quickly kill them:

    ::
    
        $ <ctrl-z>    # Suspend the current process
        $ kill %1     # Kill the suspended process
        $ jobs        # Verify the job was terminated
        
        
        








