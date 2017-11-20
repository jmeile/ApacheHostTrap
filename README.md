# ApacheHostTrap
Configuration files to ban bad hosts in apache

The following files are included:
* evil_host_trap_env.conf: sets the variable EVIL_HOST_PATH indicating the
  location of the configuration files. By default, I set it to:
  /usr/local/apache_evil_host_trap
* evil_host_trap_init.conf: initializes the bot trap engine. Here the already
  registered hosts will be banned throgh a RewriteRule. Some common exploits are
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
  * blacklisted_hosts.txt: RewriteMap for blocking access with apache
  * blacklisted_hosts_full.txt: verbose log for looking what a banned host was
    accessing, when it got banned
* blacklisted_hosts_my_list.txt: This is a list of hosts I have banned forever
  in my websites. They were accessing files that weren't present or disabled on
  my websites. You may use them if you want.
* resolve_ips_in_log: this is just a shell script to resolve the ip addresses in
  your blacklist files. This is usefull for example to make sure that you didn't 
  banned a legit service, ie: the Archive.org bot, which unfortunatelly tries to
  access xmlrpc.php
* VirtualHost_sample.conf: a short example of how to use it with an apache
  VirtualHost
  
Additionally, I also include this file:
* block_bots.conf: blocks some potentially bad bots, ie: downloaders, mail
  addresses collector programs, from accessing your website. This isn't an
  automatic approach. You will have to manually add the bot in this file.

You will find more information about using it inside the file:
evil_host_trap_init.conf

Please note that after cloning this repository, you have to set the permissions
so that the apache user can access the files, ie:
chown -R www-data:www-data /usr/local/apache_evil_host_trap
