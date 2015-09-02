===========
Shibboleth
===========

Guide for Shibboleth 2.5.2 - Apache 2.4.7 - Invenio 1.2.1
 

::

    # apt-get install libapache2-mod-shib2


    # diff shibboleth2.xml shibboleth2.xmlOrig
    24c24
    <                          REMOTE_USER="mail eppn persistent-id targeted-id" signing="true">
    ---
    >                          REMOTE_USER="eppn persistent-id targeted-id">
    36c36
    <                   checkAddress="false" handlerSSL="true" cookieProps="http">
    ---
    >                   checkAddress="false" handlerSSL="false" cookieProps="http">
    44,45c44,45
    <             <SSO
    <                  discoveryProtocol="SAMLDS" discoveryURL="https://gridp.garr.it/ds/WAYF">
    ---
    >             <SSO entityID="https://idp.example.org/idp/shibboleth"
    >                  discoveryProtocol="SAMLDS" discoveryURL="https://ds.example.org/DS/WAYF">
    69c69
    <         <Errors supportContact="admin@sci-gaia.eu"
    ---
    >         <Errors supportContact="root@localhost"
    81,83d80
    <         <MetadataProvider type="XML" uri="https://gridp.garr.it/metadata/gridp-test.xml"
    <               backingFilePath="gridp-test.xml" reloadInterval="7200">
    <         </MetadataProvider>


Copy your certificate and key into ``/etc/shibboleth`` with name ``sp.cert`` and
``sp.key`` respectively.

::

    # service shibd restart
    # a2enmod ssl
    # cd /etc/apache2/sites-available/

Edit the file ``/opt/invenio/etc/apache/invenio-apache-vhost-ssl.conf``. 

Set the variables
 ``SSLCertificateFile`` and ``SSLCertificateKeyFile`` to your certificate and key and comment/uncomment
depending on your apache version. Finally append the following to your virtual host::


        <Location "/Shibboleth.sso/">
        #   SSLRequireSSL   # The modules only work using HTTPS
        #   AuthType shibboleth
        #   ShibRequireSession On
        #   ShibRequireAll On
        #   ShibExportAssertion Off
        #   require valid-user
        #   Allow from all
           SetHandler shib
        </Location>
        <Location ~ "/youraccount/login|Shibboleth.sso/">
           SSLRequireSSL
           AuthType shibboleth
           ShibRequestSetting requireSession 1
           require valid-user
        </Location>
        Alias "/shibboleth" "/var/www/shibboleth"
        <Directory "/var/www/shibboleth">
           Options MultiViews
           AllowOverride None
           Order allow,deny
           Allow from all
        </Directory>


Enable the site:

::


    # a2ensite invenio-ssl
    # service apache2 restart



Publish the metadata of your SP in a Federation.

For GrIDP contacts are avaible in `this page <http://gridp.garr.it/contacts.html>`_



Modified the file ``access_control_config.py`` 

::

	> else:
    	    CFG_EXTERNAL_AUTH_DEFAULT = 'Local'
    	    CFG_EXTERNAL_AUTH_USING_SSO = False
    	    CFG_EXTERNAL_AUTH_LOGOUT_SSO = None
    	    CFG_EXTERNAL_AUTHENTICATION = {
    	    "Local": None,
    	    "Robot": ExternalAuthRobot(enforce_external_nicknames=True, use_zlib=False),
    	    "ZRobot": ExternalAuthRobot(enforce_external_nicknames=True, use_zlib=True)
   	    }	
        
	---
        
        < else:
            import external_authentication_sso_scigaia as ea_sso
            CFG_EXTERNAL_AUTH_USING_SSO = "SCI-GAIA"
            CFG_EXTERNAL_AUTH_DEFAULT = CFG_EXTERNAL_AUTH_USING_SSO
            CFG_EXTERNAL_AUTH_LOGOUT_SSO = 'https://oar.sci-gaia.eu/Shibboleth.sso/Logout'
            CFG_EXTERNAL_AUTH_LOGOUT_SSO = None
            CFG_EXTERNAL_AUTHENTICATION = {
            CFG_EXTERNAL_AUTH_USING_SSO : ea_sso.ExternalAuthSSOSCIGAIA(True),
            #    "Local": None,
            #    "Robot": ExternalAuthRobot(enforce_external_nicknames=True, use_zlib=False),
            #    "ZRobot": ExternalAuthRobot(enforce_external_nicknames=True, use_zlib=True)
            }

       


and added the file ``external_authentication_sso_scigaia.py``

in ``/opt/invenio/lib/python/invenio`` 

:download:`external_authentication_sso_scigaia.py <figures/external_authentication_sso_scigaia.py>`.
