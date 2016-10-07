---
date: 2016-10-07 11:40:00
title: How-to initialize PostgreSQL in OSX?
category: English
tags: CLI, database, OSX, PostgreSQL, SQL, Apple, Mac OS X, Apple, Mac OS X El Capitan
---

A little note on how I setup and bootstrap local PostgreSQL databases on OSX
machines.

Instructions below were tested on OSX 10.11 El Capitan, and target the 9.4.x series of
PostgreSQL.

1. Install and setup PostgreSQL:

    :::bash
    $ brew update
    $ brew install postgresql94 ossp-uuid

    $ mkdir -p ~/Library/LaunchAgents
    $ cp /usr/local/Cellar/postgresql94/9.4.9_1/homebrew.mxcl.postgresql94.plist ~/Library/LaunchAgents/homebrew.mxcl.postgresql.plist
    $ launchctl unload -w ~/Library/LaunchAgents/homebrew.mxcl.postgresql.plist

    $ rm -rf /usr/local/var/postgres
    $ initdb /usr/local/var/postgres -E utf8 --no-locale

    $ sed -i "s/^#port = 5432/port = 5432/" /usr/local/var/postgres/postgresql.conf
    $ sed -i "s/^#autovacuum = on/autovacuum = on/" /usr/local/var/postgres/postgresql.conf
    $ sed -i "s/timezone = 'Europe\/Paris'/timezone = 'UTC'/" /usr/local/var/postgres/postgresql.conf
    $ sed -i "s/log_timezone = 'Europe\/Paris'/log_timezone = 'UTC'/" /usr/local/var/postgres/postgresql.conf

    $ initdb /usr/local/var/postgres

    $ launchctl load -w ~/Library/LaunchAgents/homebrew.mxcl.postgresql.plist

2. Initialize PostgreSQL:

    :::bash
    $ dropdb test_db

    $ createdb -E UTF8 -T template0 test_db

    $ psql --command 'CREATE EXTENSION "citext";' --dbname=test_db
    $ psql --command 'CREATE EXTENSION "uuid-ossp";' --dbname=test_db

    $ tail -F /usr/local/var/postgres/server.log

3. Load up a dumped database:

    :::bash
    $ pg_restore --verbose --clean --no-acl --no-owner -h localhost -d test_db ~/dump/2016-10-10-test_db.dump