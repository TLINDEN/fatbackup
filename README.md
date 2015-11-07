# Fatbackup
Incremental backups without hardlinks

# What?

This is a small perl script for incremental backups with fat(32)-drives. Such filesystems don't support incremental backups using hardlinks, which most tools use with rsync. Fatbackup is a pure perl solution. Each time it is executed it saves new or changed files into a new subdirectory. It keeps track of files using a database. The restore function takes this into account and collects files of the various backup subdirectories in order to restore a snapshot. Therefore it works as it had hardlinks.

# Install

    cp fatbackup $HOME/bin


# Usage

    usage: fatbackup [-hvsidnVztrl]
    
    Backup Options:
     --include -i <dir>    Directories to backup, may occur multiple times.
     --exclude -e <regex>  Directory or file to ignore, may occur multiple times.
     --dst -d <dir>        Target directory where to save backups.
     --zip -z              Create zip archives instead of copying.
    
    Restore Options:
     --restore -r <ts>     Restore from backup made on timestamp <ts>.
     --target -t <dir>     Directory where to restore to, default origin.
    
    Lookup Options:
     --list -l [<ts>]      If no timestamp <ts> given, list snapshots (timestamps)
                           otherwise list files backed up on <ts>. if -v
                           is set, show all files of snapshot <ts>. Requires -d.
    
    General Options:
     --nodie -n            Don't exit on errors
     --help -h -?          Print help
     --version -V          Print version
     --verbose -v          Be verbose during operations
     --debug               Print debugging
    
    Notes:
     The destination directory --dst may contain format characters (see: man strftime).
     E.g. if executed daily with -d /backups/%m then 12 full backups will be made on
     the first of each month differential backups on the other days.

# Examples

## make a backup

    fatbackup -d /media/fritz.box/Intenso/desktop-%m -i $HOME/Desktop -e "\.(git|svn)" -e "(tmp|cache)" -n

Here we're saving to a cifs-mounted usb-disk with a fat32 filesystem attached to a
fritzbox. We use %m in the backup directory, so we will have a new full backup on each new
month and incremental backups on the other days. We don't save temporary and repository
stuff.

## see what's there

    fatbackup -d /media/fritz.box/Intenso/desktop-%m -l

This shows a list of what snapshots are in this backup directory. You may also peak directly into
a snapshot like this:

    fatbackup -d /media/fritz.box/Intenso/desktop-%m -l 2015-11-07-13.10

## restore a snapshot

   fatbackup -d /media/fritz.box/Intenso/desktop-%m -r 2015-11-07-13.10

Here we restore the specified snapshot back to the original source directory.

## check for errors

The backup directory contains a logfile where you can see what happened.

If you see lots of errors with "bad file descriptor" and the like and your source
filenames contain unprintable characters or :: which are not allowed on a fat(323)
filesystem, consider using zipfiles (option -z)

# Getting help

Although I'm happy to hear from fatbackup users in private email,
that's the best way for me to forget to do something.

In order to report a bug, unexpected behavior, feature requests
or to submit a patch, please open an issue on github:
https://github.com/TLINDEN/fatbackup/issues.

# Copyright and license

This software is licensed under the GNU GENERAL PUBLIC LICENSE version 3.

# Authors

T.v.Dein <tom AT vondein DOT org>

# Project homepage

https://github.com/TLINDEN/fatbackup
