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


SVN - Set Executable Bit
========================

SVN does not notice mode changes to a file once it is checked in.  
Run the following command to make a file executable in SVN.  
Requires a commit after the change.

    ::
    
        svn ps svn:executable yes <filename>
        

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


Find and edit with vim
======================

Refactoring can be a challenge from the command line.  Here is a quick way in bash to edit
a bunch of files based on the find command:

    ::
        
        vim $(find . -type d -name .svn -prune -o -type f | xargs grep db-partition | cut -f1 -d':')
        
        
Use :w and :bd to cycle thru all of the files.


Django Management on New Servers
================================

Django frustatingly fails quietly when imports fail (psycopg2).  To determine what is going on run the 
following command to check the configuration.

    ::
    
        manage.py check --verbosity=3
        
You will want to fully qualify manage.py or prefix with ``python manage.py`` if it isn't already in your path and executable.


Django: Bad Request (400)
=========================

Django requires the ``ALLOWED_HOSTS = [ '*' ]`` setting if ``Debug = False`` in ``settings.py``.  It might not be obvious 
if the ``DEBUG`` setting is casually changed.

https://docs.djangoproject.com/en/1.6/ref/settings/#std%3asetting-ALLOWED_HOSTS


Merge Directories in OS X
=========================

Merging directories in OS X is never easy.  The ``ditto`` command may be the answer:

    ::
    
        ditto -V ~/Desktop/Test ~/Downloads/Test
        
        
**Note:** Ditto will overwrite files in the destination directory with the files in the source directory.  Therefore 
it might be worth using rsync instead.  Maybe a future entry!
    
    
Reference: http://www.howtogeek.com/198043/how-to-merge-folders-on-mac-os-x-without-losing-all-your-files-seriously/


Unzip Multiple Files
====================

For some reason the unzip utility does a bad job of extracting multiple files.  Here is how to do it:

    ::
    
        for file in `ls *.zip`; do unzip $file -d `echo $file | cut -d . -f 1`; done
        
This will extract each zip file into a directory of its own.


Red Hat Password Aging
======================

Check the age of a password with the following command:

    ::
    
        sudo chage -l <user>
        
A script to check the age and warn would be helpful.


OS X Yosemite - Apple RAID Card
===============================

My 2008 Mac Pro with an Apple RAID card still seems to be plugging along.  Yosemite seems painfully slow on 
it however.  The RAID Utility has moved from ``Macintosh HD > Applications > Utilities`` to 
``Macintosh HD > System > Library > CoreServices > Applications``

http://www.macstrategy.com/article.php?127


OS X - Software Update from the Command Line
============================================

To update the software on a Mac from the command line use ``softwareupdate``.

From Apple: http://support.apple.com/kb/HT200113

The ``softwareupdate`` command is also available in OS X client version of the operating system and can be used remotely if Screen Sharing, Remote Login or Remote Management is enabled in the Sharing pane of System Preferences.

First, connect to the remote server using SSH, or connect to it via screen sharing and open a Terminal window.

In OS X Lion and later, the ``softwareupdate`` command must be run as root and some options require it in earlier versions of OS X, so you should start by using the sudo command to enter a root shell:

    ::
        
        sudo -s
        
You will need to enter an administrator's password when prompted.

You can use the ``-l`` or ``--list`` argument to see which updates are available.

    ::
    
        softwareupdate --list
        
This will return a list like the following.

    ::

        Software Update found the following new or updated software:
        * Safari5.1.5Lion-5.1.5
        Safari (5.1.5), 45266K [recommended]
        * iTunesX-10.6.1
        iTunes (10.6.1), 127659K [recommended]
        Updates that require a restart will be marked with [restart].

You can use the ``-i`` or ``--install`` argument to install one or more of the available updates. For instance, to install the Safari and iTunes updates listed above, use this command:

    ::
        
        softwareupdate --install Safari5.1.5Lion-5.1.5 iTunesX-10.6.1

Or you can use the ``-a`` or ``--all`` argument to install all available updates:

    :: 
        
        softwareupdate --install --all
        
The Software Update tool will report progress as it downloads and installs the updates. When it is finished you can either use the exit command to exit your root shell, or the reboot command to restart the server (if required by the update.)

To see more options and usage instructions, type:

    ::
        
        man softwareupdate

Docker Container Management
===========================

One liner to stop / remove all of Docker containers:

    ::

        docker stop $(docker ps -a -q)
        docker rm $(docker ps -a -q)
        
https://coderwall.com/p/ewk0mq?&p=7&q=

Automount Network Directories on OS X
=====================================

To make use of space that may be available on other network devices such as a Time Capsule automount the 
volume using ``automount``.

Create a directory for the destination share in your User directory ``/home/<user>/Share``.

Add a line to the ``/etc/auto_master`` file:
 
    ::

        # /etc/auto_master
        #
        # Automounter master map
        #
        +auto_master                        # Use directory service
        /Users/User/Share           auto_nas
        /net                                -hosts      -nobrowse,hidefromfinder,nosuid
        /home                               auto_home   -nobrowse,hidefromfinder
        /Network/Servers                    -fstab
        /-                                  -static

Create the file ``/etc/auto_nas`` containing the following line:

    ::
        
        # /etc/auto_nas
        Shared_Folder -fstype=afp afp://User:Password@ip/Shared_Folder

Run the following commnad: ``sudo automount -vc`` in Terminal.  The output should be similar to the following:

    ::
    
        $ sudo automount -vc
        automount: /Users/User/Share updated
        automount: /net updated
        automount: /home updated
        automount: no unmounts

The line containing ``automount: /Users/User/Share updated`` is the clue that everything worked

Both files may need a trailing empty line or it won't work and you'll get the following error:
automount[pid]: map /etc/auto_master: line too long (max 4095 chars) or automount[pid]: map /etc/auto_nas: line too long (max 4095 chars)

Tested and verified with OS X Yosemite (10.10).

To unmount a remotely mounted volume:

    ::
        
        $ umount /Users/User/Share 
        
To discover what volumes are currently mounted run the command:

    ::
        
        $ mount
        
Reference: http://apple.stackexchange.com/questions/152712/can-mount-afp-share-via-mount-afp-but-not-via-autofs
http://useyourloaf.com/blog/2011/01/24/using-the-mac-os-x-automounter.html
http://hints.macworld.com/article.php?story=2001120201020569

PostgreSQL Common Table Expressions (CTE)
=========================================

PostgreSQL does not have Colloections and BULK INSERT as Oracle does.  Use CTEs instead and the WITH clause!

http://beyondrelational.com/modules/2/blogs/80/posts/10748/day-6-common-table-expressions-using-with-clause-in-postgresql.aspx
http://www.the-art-of-web.com/sql/upsert/
http://stackoverflow.com/questions/18439054/postgresql-with-delete-relation-does-not-exists
http://stackoverflow.com/questions/17575489/postgresql-cte-upsert-returning-modified-rows
http://www.postgresql.org/docs/9.3/static/queries-with.html
http://stackoverflow.com/questions/8721503/in-postgres-sql-guidance-on-using-the-with-clause
http://blog.heapanalytics.com/dont-iterate-over-a-postgres-array-with-a-loop/
http://tiku.io/questions/3923973/postgresql-equivalent-of-oracle-bulk-collect
http://www.postgresql.org/docs/8.3/static/plpgsql-porting.html
http://stackoverflow.com/questions/5531660/type-type-name-is-table-of-number-index-by-varchar264-from-oracle-in-postgresq
http://www.postgresmigrations.com/pdf/Three%20Array%20Type%20Overview.pdf
http://stackoverflow.com/questions/22339628/cursor-based-records-in-postgresql
http://www.postgresql.org/docs/current/interactive/plpgsql-control-structures.html#PLPGSQL-STATEMENTS-RETURNING
http://www.postgresql.org/docs/current/interactive/plpgsql-control-structures.html#PLPGSQL-FOREACH-ARRAY
http://stackoverflow.com/questions/19145761/postgres-for-loop
http://stackoverflow.com/questions/8674718/best-way-to-select-random-rows-postgresql
http://stackoverflow.com/questions/7943233/fast-way-to-discover-the-row-count-of-a-table/7945274#7945274
http://stackoverflow.com/questions/3873514/what-is-the-equivalent-of-pl-sql-notfound-in-pl-pgsql
http://www.linuxtopia.org/online_books/database_guides/Practical_PostgreSQL_database/PostgreSQL_x20238_002.htm
http://www.postgresql.org/docs/9.4/static/sql-select.html
