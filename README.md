# CSEC302-Demo-Tommy
CVE-2022-21661 WordPress SQL Injection Vulnerability

It is common for WordPress plugins to be vulnerable, but what is so special about this vulnerability is that it is actually in WordPress itself.
Since this vulnerability is in WordPress, you will likely need to install xampp or some other Apache web server in order to run the php files and set up WordPress.
The vulnerable code within WordPress is however still accessed through the use of plugins.




# Resources:
## Main instructions and proof of concept
https://www.zerodayinitiative.com/blog/2022/1/18/cve-2021-21661-exposing-database-info-via-wordpress-sql-injection
## Old vulnerable WordPress
https://github.com/WordPress/WordPress/tree/4ec018ad06825987926221d5a89ad482d0836e85
## Current WordPress
https://github.com/WordPress/WordPress
## Installing WordPress and setting it up (I needed xampp to run the php files)
https://wordpress.org/support/article/how-to-install-wordpress/
https://blog.devsense.com/2021/configuring-xampp
## Misc
https://github.com/TAPESH-TEAM/CVE-2022-21661-WordPress-Core-5.8.2-WP_Query-SQL-Injection
