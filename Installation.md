# POOKMAIL INSTALLATION #


## SYSTEM REQUIREMENTS ##
  1. Apache (>1.3.37)
  1. MySQL (>3.23.56)
  1. PHP (>4.4.4)
  1. Perl 5.8.8
    1. Mail::Message (2.080)
    1. DBI
    1. Digest::MD5
    1. Net::SMTP::Server

## CAPTCHA REQUIREMENTS (OPTIONAL) ##
  1. ZLib 1.2.3
  1. FreeType 2.1.10
  1. LibJPEG 6b
  1. LibPNG 1.2.8
  1. GD 2.0.33


## Installation ##

  1. Checkout PookMail source code
```
   $ svn checkout http://pookmail.googlecode.com/svn/trunk/ pookmail
```
  1. Create the PookMail database
    1. Add 'pookmail' user to your Mysql
```
      $ /usr/local/mysql/bin/mysql --user=root mysql
      mysql> GRANT SELECT,INSERT,UPDATE,DELETE,CREATE,DROP 
		ON gatika.* TO 'pookmail'@'localhost'
		IDENTIFIED BY '123456';
```
    1. Create the Database schema:
```
      $ mysql -u pookmail -p gatika -h localhost < pookmail/db/gatika.db
```
  1. Configure the SMTP Server
    1. Edit the pookmail/bin/Configure.pm file, and customize the following parameters to suits to your system:
```
      smtp => {
          ip => "0.0.0.0",
          port => 25
      },
      timeout => 45,
      log_dir => "/path/to/pookmail/log/",
      db => {
         host => "localhost",
         user => "pookmail",
         pass => "123456",
         db   => "gatika"
      }
```
    1. Startup the STMP Server
```
      $ cd pookmail/bin
      $ ./smtp.pl &
```
    1. Configure the automatic emails deletion
```
      $ crontab -l

      # m h  dom mon dow   command
      59 * * * * cd /path/to/pookmail/bin/; ./cleanup.pl
```
  1. Configure the WEB interface
    1. Customize the Pookmail Apache configuration file named 'pookmail/conf/httpd.conf' to suits your system:
```
      ServerAdmin info@your_domain_here
      DocumentRoot /path/to/pookmail/htdocs/
      ServerName www.your_domain_here
      ServerAlias your_domain_here

      php_value include_path ".:/path/to/pookmail/htdocs/"
      php_value session.save_path "/path/to/pookmail/log/"
```
    1. Add the following line to your current Apache 'httpd.conf' file:
```
      Include /path/to/pookmail/conf/httpd.conf
```
    1. Restart your Apache
    1. Configure the web interface configuration file named 'pookmail/htdocs/config.php
```
      $config['db_user']='pookmail';
      $config['db_pwd'] ='123456';
      $config['db_db']  ='gatika';
      $config['db_host']='localhost';

      $config['logfile']='/path/to/pookmail/log/webusage_log';
```
  1. Ready! You should be able to access to the web interface.