=============
OAR - DOI/PID
=============

If you would like to change the DOI/PID Prefix


::

	cd /opt/invenio/var/www/form
	sudo vim request_doi.py


::


	#!/usr/bin/env python

	import json,cgi,time
	import httplib2, sys, base64, codecs

	res=[]
	retCode=0
	errCode=''
	doi='11623' 
	res =  "%s/sci-gaia:%s" % (doi,time.time())
	print "Content-type: application/json\n\n"
	print json.dumps(res)



11623/<repo-name>/<unique-id>




template email 

*Send to*: <handles@sci-gaia.eu>

*Subject*: OAR Repo-NAME - new PID

::

	Dear Handle Server Administrators,

	Could you please register the PID of the following resource?

	CREATE 11623/repo-name:<unique-id>
	100 HS_ADMIN 86400 1110 ADMIN 300:111111111111:0.NA/11623
	2 URL 86400 1110 UTF8https://repo-name/record/id  
	3 DESC 86400 1110 UTF8 Title-record

	Best regards,

	The Librarian of the "Repo-NAME" Oper Access Repository


