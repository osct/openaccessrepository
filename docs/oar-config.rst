=================
OAR Configuration
=================

*How To Change Site URL*


1) If running you "Stop" bibsched queue so that no jobs are running during migration.

::

	cd /opt/invenio/bin/
        sudo -u www-data ./bibsched
        press "A"



2) Edit your invenio-local.conf and put wanted values there:

::

	
	$ sudo -u www-data vim /opt/invenio/etc/invenio-local.conf # edit as follows



3) Propagate these changes to all installed files:

::


	$ sudo -u www-data /opt/invenio/bin/inveniocfg --update-all



4) Update Apache configuration file, either by running:

::


	$ sudo -u www-data /opt/invenio/bin/inveniocfg --create-apache-conf


or by manually editing virtual host configuration files 

::


	sudo -u www-data vim /opt/invenio/etc/apache/invenio-apache-vhost*.conf.




5) You can restart your Apache server now:

::


	$ sudo /etc/init.d/apache2 restart


6) Remove help pages (user|admin|hacking) cache (please first ensure that you have not mistakenly edited these files to add custom information, instead of editing the source of the help pages):

::


	$ sudo -u www-data rm -r /opt/invenio/var/cache/webdoc/

(Cache will be automatically recreated based on the source file when one accesses a page. 
You can force the creation of these pages by accessing the table of content for each section: http://yoursite.eu/help/contents, http://yoursiste.eu/help/admin/contents and http://yoursite.eu/help/hacking/contents)

7) Put your bibsched queue back to automatic mode, and you are done.

::

	cd /opt/invenio/bin/
        sudo -u www-data ./bibsched
        press "A"
