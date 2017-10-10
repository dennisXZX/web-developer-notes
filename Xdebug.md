## Xdebug

### Install Xdebug on PhpStorm

- run `brew install php<php version>-xdebug`, you can use `brew search xdebug` to look for the package available on brew

- run `php -v` to make sure Xdebug is successfully installed

```
Copyright (c) 1997-2016 The PHP Group
Zend Engine v2.6.0, Copyright (c) 1998-2016 Zend Technologies
    with Xdebug v2.5.5, Copyright (c) 2002-2017, by Derick Rethans
```

- run `php --ini` to check the location of the Xdebug config file

```
Configuration File (php.ini) Path: /usr/local/etc/php/5.6
Loaded Configuration File:         /usr/local/etc/php/5.6/php.ini
Scan for additional .ini files in: /usr/local/etc/php/5.6/conf.d
Additional .ini files parsed:      /usr/local/etc/php/5.6/conf.d/ext-xdebug.ini
```

- edit `ext-xdebug.ini` to make sure it looks like the following

```
[xdebug]
zend_extension="/usr/local/opt/php56-xdebug/xdebug.so"
xdebug.remote_enable=on
xdebug.remote_connect_back=on
```
