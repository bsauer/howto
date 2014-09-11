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

SVN - Determine current branch
==============================

Run the following command to display the current SVN branch information:

    ::
    
        svn info
        
The next bashrc update add something like this to put branch information the prompt:

http://petdance.com/2013/04/my-bash-prompt-with-gitsvn-branchstatus-display/

        
        
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
        

Checking Machine Information:
=============================

Linux:
------

    http://docs.oracle.com/cd/E23824_01/html/821-1451/gkkwk.html


Solaris:
--------
        
    http://docs.oracle.com/cd/E23824_01/html/821-1451/gkkwk.html


Windows 7 - Minimized windows
=============================

Windows sometimes forgets the size of a window and it opens too small to even show up on the desktop. 
Windows 7 has a cool new feature that should help.  Click the icon in the taskbar to ensure that 
the program has focus. Then hold down the Windows key and press the right-arrow a few times. That 
should move the window across your screens and eventually bring it back onto the screen that is still 
active.  

http://social.technet.microsoft.com/Forums/windows/en-US/f3040564-0457-4c91-af71-dce1bc673a99/moverecover-offscreen-window


vim :w!!
========

Place the following in your .vimrc file:

::

    cmap w!! %!sudo tee > /dev/null %
    
Then when you need to make changes to a system file, you can 
override the read-only permissions by typing ``:w!!``, vim will ask for 
your sudo password and save your changes.

