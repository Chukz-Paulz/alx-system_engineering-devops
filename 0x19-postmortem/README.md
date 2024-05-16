0x19-postmortem

Postmortem
Upon the release of Holberton School's System Engineering & DevOps project 0x19 at approximately 00:07 Pacific Standard Time (PST), an outage was detected on an isolated Ubuntu 14.04 container running an Apache web server. GET requests to the server were returning 500 Internal Server Errors instead of the expected HTML file for a simple Holberton WordPress site.

Debugging Process
Bug debugger Brennan (BDB... yes, those are my initials... clever, right?) discovered the issue upon opening the project around 19:20 PST and was promptly tasked with resolving it.

Checked running processes: Used ps aux to verify that two Apache processes—root and www-data—were running correctly.

Examined the server configuration: Inspected the sites-available folder in the /etc/apache2/ directory and confirmed that the web server was serving content from /var/www/html/.

Used strace for debugging: In one terminal, ran strace on the PID of the root Apache process and in another terminal, executed a curl command on the server. Despite high expectations, strace provided no useful information.

Repeated strace on www-data process: With tempered expectations, ran strace on the PID of the www-data process while curling the server. This time, success! Strace revealed a -1 ENOENT (No such file or directory) error when attempting to access /var/www/html/wp-includes/class-wp-locale.phpp.

Identified the erroneous file extension: Searched through the files in the /var/www/html/ directory using Vim pattern matching and located the erroneous .phpp extension in wp-settings.php (Line 137: require_once( ABSPATH . WPINC . '/class-wp-locale.php' );).

Fixed the typo: Removed the trailing p from the file name.

Tested the fix: Executed another curl command on the server, resulting in a successful 200 OK response!

Automated the fix: Created a Puppet manifest to automate the correction of this error.

Summation
The root cause was a typo in the WordPress application code. Specifically, wp-settings.php was attempting to load class-wp-locale.phpp instead of class-wp-locale.php. The fix involved correcting the typo.

Prevention
This was an application error, not a web server error. To prevent similar issues in the future:

Test thoroughly: Always test the application before deployment. This error would have been caught and resolved earlier with proper testing.
Implement status monitoring: Use an uptime-monitoring service like UptimeRobot to receive instant alerts in case of website outages.
In response to this incident, I wrote a Puppet manifest (0-strace_is_your_friend.pp) to automate the correction of any similar errors by replacing .phpp extensions in /var/www/html/wp-settings.php with .php.

Of course, such errors will never happen again because we're programmers, and we never make mistakes! 😉
