### rsyncas - Rsync Auto Script ###
Yes, i was running out of ideas for the name

---

### License ###
This software is licensed under the highly restrictive <a href="http://en.wikipedia.org/wiki/WTFPL">DO WHAT THE FUCK YOU WANT TO PUBLIC LICENSE 2.0</a> or later.
If this license doesn't suit you: you're doing it wrong.

---

### What it does? ###
Sync files. Yes its just a rsync automator.
I use it to backup my servers

---

### There are so many backup solutions/scripts, why another? ###
* This was a 2 hours cooking to replace some bloated "backup solution/script" that i found on the "internets" and was burning my CPU.
* This one DON'T ask your for some oldschool tape drive.
* This one does exactly what i want.

---

### Installing dependencies (as root)
* On Debian: __apt-get install rsync php5-cli__
* On CentOs: __yum install rsync php5-cli__

---

### Installing rsyncAs
    curl https://github.com/hugorodrigues/rsyncas/raw/master/rsyncas >> /usr/bin/rsyncas
    curl https://github.com/hugorodrigues/rsyncas/raw/master/rsyncas.example.conf >> /etc/rsyncas.conf
    vim /etc/rsyncas.conf

__Note:__ You can use/install rsync without root, just change /usr/bin/rsyncas to ~/rsyncas/rsyncas and /etc/rsyncas.conf to ~/rsyncas/rsyncas.conf

---

### Upgrading rsyncAs
    curl https://github.com/hugorodrigues/rsyncas/raw/master/rsyncas >> /usr/bin/rsyncas

---

### Basic usage ###
First you define a backup setting like:

    $bck['mybackup'] = array(
        'description' => 'This is the description of your backup conf',
        'pre_cmd' => 'someCommandToExecute_BEFORE_Sync.sh',
        'source' => '/my/local/folder/to/backup/',
        'destination' => 'someUser@someBackupServer.com:/some/folder/',
        'post_cmd' => 'someCommandToExecute_AFTER_Sync.sh'
    );

and then to backup that setting you run:

    $ rsyncas mybackup

You can have multiple settings, and run them all:

    $ rsyncas --all

You may also define setting in runtime:

    $ rsyncas source='/some/folder/' destination='someUser@someBackupServer.com:/some/folder/' description='My descrition'

---

### Some real world examples ###

__Backup a webserver:__

    $bck['www'] = array(
        'description' => 'Backup http root',
        'source' => '/var/www/',
        'destination' => 'someUser@someBackupServer.com:/backups/www/'
    );

To backup this "www" setting:

    $ rsyncas www

__Backup a Mysql Server:__

    $bck['mysql'] = array(
        'description' => 'Mysql Backup',
        'pre_cmd' => 'mysqldump --opt mydatabase > /tmp/mysql/mydatabase.sql',
        'source' => '/tmp/mysql/',
        'destination' => 'someUser@someBackupServer.com:/backups/mysql/',
        'post_cmd' => 'rm /tmp/mysql/mydatabase.sql'
    );

To backup this "mysql" setting:

    $ rsyncas mysql

__Mirror a website:__

    $bck['mirror'] = array(
        'description' => 'Server Mirror',
        'source' => '/var/www/',
        'destination' => 'someUser@someMirrorServer.com:/var/www/'
    );

To backup this "mirror" setting:

    $ rsyncas mirror


__To backup all the previous defined settings:__

    $ rsyncas --all

---

### Cron usage ###
Just like you normally do. Something like:

    10 2 * * * root rsyncas demoName

---

### Man ###
    ---------------------------------------------------------------------------
    rsyncas version 0.1  |  https://github.com/hugorodrigues/rsyncas
    ---------------------------------------------------------------------------
    rsyncas comes with ABSOLUTELY NO WARRANTY.  This is free software, and you
    are welcome to redistribute it and/or modify it under the terms of the
    Do What The Fuck You Want To Public License, Version 2.
    ---------------------------------------------------------------------------

    Usage: rsyncas --help         | useless. shows this message
      or   rsyncas --all          | run all pre-defined configurations
      or   rsyncas --test         | Test e-mail settings/delivery
      or   rsyncas key            | run a specific setting
      or   rsyncas [OPTION]       |

    Options:
      description=                | description of the sync
      source=                     | source target
      destination=                | destination target
      ssh_port=                   | alternative ssh port
      pre_cmd=                    | command to execute before sync
      post_cmd=                   | command to execute after sync

    Examples:
      rsynca mybackup             | run the pre-defined "mybackup" configuration
      rsynca --all                | run all pre-defined configurations

---

### Requirements ###
* rsync - http://samba.anu.edu.au/rsync/
* php-cli  - http://php.net