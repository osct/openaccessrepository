===========
Shibboleth
===========


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


Copy your certificate and key into `/etc/shibboleth` with name `sp.cert` and
`sp.key` respectively.

::

    # service shibd restart
    # a2enmod ssl
    # cd /etc/apache2/sites-available/

Edit the file `/opt/invenio/etc/apache/invenio-apache-vhost-ssl.conf`. Set the variables
`SSLCertificateFile` and `SSLCertificateKeyFile` to your certificate and key and comment/uncomment
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



Publish the metadata of your SP in a Federation. For GrIDP contact .....



Modified the file `access_control_config.py` and created the file `external_authentication_sso_scigaia.py`
in `/opt/invenio/lib/python/invenio`
