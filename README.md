mysqlmigrate
============

A tool for quickly and routinely migrating MySQL databases.

SYNOPSIS
========

    mysqlmigrate [OPTION]... FROM_DB TO_DB [FIND_REGEX] [REPLACE]

    mysqlmigrate [OPTION]... FILE TO_DB [FIND_REGEX] [REPLACE]

    mysqlmigrate --dump [OPTION]... FROM_DB

OPTIONS SUMMARY
===============

Here is a short summary of the options available.

    -h, --help                  show help
    -v, --version               show the version
    -t, --test                  run in test mode (prints config and commands)
    -d, --dump                  only do a database dump
    --innodb-file-format FORMAT set the innodb_file_format (i.e. Barracuda or Antelope)
                                NOTE: this sets it globally which will stick and affect the entire server
    --innodb-file-per-table ON  sets innodb_file_per_table (can be ON or OFF)
                                NOTE: this sets it globally which will stick affect the entire server
    --ignore-definer            Do not import DEFINER statements
    --net-buffer-length         sets net_buffer_length in bytes (1000000 is 1 MB)
    --max-allowed-packet        sets max_allowed_packet in bytes (1000000000 is 1 GB)
    -s, --source FILE           use a source file instead of pulling FROM_DB
    -u, --user USER             the FROM_DB and TO_DB user
    -p, --password PASS         the FROM_DB and TO_DB password
    --from-user USER            the FROM_DB user
    --from-password PASS        the FROM_DB password
    --from-host HOST            the FROM_DB host
    --to-user USER              the TO_DB user
    --to-password PASS          the TO_DB password
    --to-host HOST              the TO_DB host

USAGE
=====

By default, mysqlmigrate will drop the entire database of the destination. So, to migrate from a remote database to a local database:

    mysqlmigrate --from-user root --from-password password --from-host user@remoteserver.com --to-user root --to-password password database_name database_name

You'll notice that a file "database_name.sql" is created in the directory you are currently in. The next time you run the above command it will read from that file instead of grabbing the database again.

To migrate to a database from a file:

    mysqlmigrate -u root -p password database_name.sql database_name

This is useful if, for example, you want to restore from an old backup like so:

    mysqlmigrate -u root -p password database_name-20131203.sql database_name

To simply run a mysqldump:

    mysqlmigrate -u root -p password -h user@remoteserver.com -d users

There are cases where the database you are migrating is too large and you need to use Barracuda, so do this:

    mysqlmigrate --from-user user --from-password password --from-host user@remoteserver.com --to-user root --to-password password --innodb-file-format Barracuda --innodb-file-per-table ON --net-buffer-length 1000000 --max-allowed-packet 1000000000 database_name database_name

And, if you need to ignore the DEFINER statements:

          mysqlmigrate --ignore-definer --from-user user --from-password password --from-host user@remoteserver.com --to-user root --to-password password --innodb-file-format Barracuda --innodb-file-per-table ON --net-buffer-length 1000000 --max-allowed-packet 1000000000 database_name database_name

Tip: if you like backups, make an alias "[mvbup](https://github.com/josephdpurcell/dotfiles/blob/master/.bash_includes/aliases#L162)" to quickly move the "database_name.sql" files to "database_name-YYYYMMDD.sql".

SEE ALSO
========

mysqldump

LICENSE
=======

Copyright 2014 Joseph D. Purcell.

This program is free software; you can redistribute it and/or modify it under the terms of the MIT License. See LICENSE.txt for details. This script will hopefully be useful, but comse WITHOUT ANY WARRANTY, nor the implied warranty that it is useful to anyone.

https://github.com/josephdpurcell/mysqlmigrate

