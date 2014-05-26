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

        rsync -avxP --delete /path/to/directory/to/backup /path/to/directory/for/storing/bakcup

        -a means archive mode
        -v means print out the verbose information during backuping
        -x means not cross filesystem boundaries
        -P is shortcut for
            --progress means printing information showing the progress of the transfer
            and
            --partial means keeping partially transferred files if the transfer is interrupted
 

Reference: http://yyab.wordpress.com/2006/12/18/resume-a-large-scp-transfer/


Undo SVN Add
============

Adding new files and directories to SVN can sometimes include files and directories that should not be
included.  Remove them via the following command:

    ::
    
        svn rm --keep-local FILENAME
        
Bash Setting and DOT (.) Files
==============================

GitHub has some good information on setting up dot files.

http://dotfiles.github.io







