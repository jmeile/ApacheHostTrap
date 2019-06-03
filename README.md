# ApacheHostTrap
Configuration files to ban bad hosts in Apache 2.4

## Getting Started
* First go to the root folder where you want to put your files, ie:
```
cd /usr/local
```

* Clone the repository:
```
git clone https://github.com/jmeile/ApacheHostTrap
```
  It will be downloaded into a folder called: ApacheHostTrap

* Edit the file evil_host_trap_env.conf and set the EVIL_HOST_PATH variable to
  the path that you used for cloning the repository, ie:
```
/usr/local/ApacheHostTrap
```
  You may also need to setup the APACHE_LOG_DIR if your apache distribution
  doesn't do this by default.

* Include the file: evil_host_trap_env.conf somewhere in your apache
  configuration files, ie: do a symlink to it under /etc/apache2/conf-enabled

* Use the VirtualHost_sample.conf to setup your own VirtualHosts. You may
  accomodate it to your needs. The really important thing is to include:
  * evil_host_trap_init.conf
  * Either: evil_host_trap_no_wp.conf or evil_host_trap_wp.conf
  * evil_host_trap_register.conf
  In that order

* Edit the script: check_banned_host and set the following variables according
  to your needs:
  * BANNED_HOSTS_PATH: Where the text file containing the whitelisted and banned
    hosts is located, ie:
```
/usr/local/ApacheHostTrap/blacklisted_hosts.txt
```
  * UNBAN_TIME: Time in seconds to ban the ip, ie: 86400 will ban the hosts for
    one day.

* You may also want to edit the files: evil_host_trap_no_wp.conf and
  evil_host_trap_wp.conf according to your needs, ie: add more exploits. You can
  also add new files, ie: evil_host_trap_jombla.conf with common scans for this
  cms.

* If you are using the block_bots.conf file, then you may also add or remove
  some user agents there.

## Some more detailed documentation
Note: this may work in Apache 2.2, but you will have to manually install the
mod_define module, which can be downloaded from here:
https://people.apache.org/~rjung/mod_define

I tried that module, but somehow I didn't manage to get it working with Apache
2.2. At the end, I replaced all the ${APACHE_LOG_DIR} and ${EVIL_HOST_PATH}
by the real paths. So, I guess you'd better use Apache 2.4. I have some lines
indicated the way of doing it with Apache 2.2; however it wan't tested there,
you may used it under your own risk.

For Apache 2.2. you have to define the variable APACHE_LOG_DIR (see the file:
evil_host_trap_env.conf). Apche 2.4 already does this by default.

The following files are included:
* evil_host_trap_env.conf: sets the variable EVIL_HOST_PATH indicating the
  location of the configuration files. By default, I set it to:
  /usr/local/ApacheHostTrap
* evil_host_trap_init.conf: initializes the bot trap engine. Here the already
  registered hosts will be banned through a RewriteRule. Some common exploits are
  also defined here.
* evil_host_trap_wp.conf and evil_host_trap_no_wp.conf: files with some common
  exploits for WordPress and non WordPress websites. Please note that here I
  don't contemplate any other CMS like Jombla or Typo, so, if you want to add
  them, then you have to add your own evil_host_trap_\*.conf files. Also note
  that in the case of WordPress, I assume that xmlrpc.php is disabled. If you
  are working with this, then you will have either to whitelist your ips or
  completely remove the RewriteRule in evil_host_trap_wp.conf
* evil_host_trap_register.conf: here the bad hosts will be logged into two
  files:
  * blacklisted_hosts.txt: text file with the whitelisted and banned hosts 
  * blacklisted_hosts_full.txt: verbose log for looking what a banned host was
    accessing, when it got banned
* check_banned_host: checks the ips of the hosts accessing your site and see if
  they are either whitelisted, banned, or are accessing forbidden resources.
* resolve_ips_in_log: this is just a shell script to resolve the ip addresses in
  your blacklist files. This is usefull for example to make sure that you didn't 
  banned a legit service, ie: the Archive.org bot, which unfortunatelly tries to
  access xmlrpc.php
* VirtualHost_sample.conf: a short example of how to use it with an apache
  VirtualHost
  
Additionally, I also include this file:
* block_bots.conf: blocks some potentially bad bots, ie: downloaders, mail
  addresses collector programs, from accessing your website. This isn't an
  automatic approach. You will have to manually add the bot in this file. Note
  that this file also uses Apache 2.4 syntax. To make it compatible with Apache
  2.2 you will have to read the comments inside the file.

You will find more information about using it inside the file:
evil_host_trap_init.conf

Please note that after cloning this repository, you have to set the permissions
so that the apache user can access the files, ie:
chown -R www-data:www-data /usr/local/ApacheHostTrap

Also the scripts: check_banned_host and resolve_ips_in_log must have excecution
permissions
