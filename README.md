# CSEC302-Demo-Tommy
CVE-2022-21661 WordPress SQL Injection Vulnerability

It is common for WordPress plugins to be vulnerable, but what is so special about this vulnerability is that it is actually in WordPress itself.
Since this vulnerability is in WordPress, you will likely need to install xampp or some other Apache web server in order to run the php files and set up WordPress.
The vulnerable code within WordPress is however still accessed through the use of plugins.

The vulnerability in the code is found in the WP_Query class within WordPress. This class takes in queries from plugins and forms SQL statements using that information. However, the code was found to be vulnerable as the input was not sanitized properly.

The string of code leading to the vulnerability begins in wp-admin/admin-ajax.php. In this file is code that checks to see if the user is currently logged in and authenticated. If the user is not logged in 
If the user is not authenticated, they get sent to the plugin, found at wp-content/plugins/ele-custon-skin/includes/ajax-pagination.php. This then calls the action 'get_document_data,' which creates a WP_Query object.

Once a WP_Query object is created, the get_posts method is called in wp-includes/class-wp-query.php. Within the get_posts method, the get_sql method is called, leading to the formation of an SQL statement. Within the get_sql method is the get_sql_for_clause which calls to clean_query, which is supposed to clean the user input. However, the method fails to properly validate the terms parameter if the taxonomy parameter is empty and the field paramter is "term_taxonomy_id." This would then allow the user input from the terms parameter to be returned from the clean_query method and gets added to an SQL statement. This would allow for the user to enter malicious SQL statements and gain user data from the system.

Using the following post statement, the user could bypass the term query being sanitized and perform SQL injection.

This was fixed in later versions by changing the cleaning from 

$query['terms'] = array_unique( (array) $query['terms'] );

to

if ( 'slug' === $query['field'] || 'name' === $query['field'] ) {
			$query['terms'] = array_unique( (array) $query['terms'] );
		} else {
			$query['terms'] = wp_parse_id_list( $query['terms'] );
		}


Below is a POST Statement that could be used to perform the SQL Injection:

POST /WordPressVuln/wp-admin/admin-ajax.php HTTP/1.1
Host: localhost
Upgrade-Insecure_Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:95.0) Gecko/20100101 Firefox/95.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.99
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: cross-site
Sec-Fetch-User: ?1
Cache-Control: max-age=0
Connection: close 
Content-Type: application/x-www-form-urlencoded
Content-Length: 118

action=<action_name>&nonce=a85a0c3bfa&query_vars={"tax_query":{"0":{"field":"term_taxonomy_id","terms":["<inject>"]}}}

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
