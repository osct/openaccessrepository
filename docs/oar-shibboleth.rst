===================================
External Authentication: Shibboleth
===================================



.. sidebar:: Version Available 1.0
    :subtitle: External Authentication: Shibboleth

    - Shibboleth 2.5.2
    - Apache 2.4.7 
    - `Invenio 1.2.1 <http://invenio-software.org/>`_



Say your institution has setup Single Sign-On solution based on Shibboleth. 

Here are the steps to follow in order to integrate Shibboleth with Invenio 1.2.1  as a Service Provider.


**Install the following packages**


::

    # apt-get install libapache2-mod-shib2



**Configure shibboleth**

Modified file ``/etc/shibboleth/shibboleth2.xml``



::


    # diff /etc/shibboleth/shibboleth2.xml  
    23c23,24c24,
    <                          entityID="https://oar.sci-gaia.eu/shibboleth"  attributePrefix="ADFS_"
    <                          REMOTE_USER="mail eppn persistent-id targeted-id" signing="true">
    ---
    >                          entityID="https://example.com/shibboleth"
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



Modified the file ``/etc/shibboleth/attribute-map.xml``

Uncomment LDAP-based attributes

::

		<!-- Examples of LDAP-based attributes, uncomment to use these... -->
		<Attribute name="urn:mace:dir:attribute-def:cn" id="cn"/>
		<Attribute name="urn:mace:dir:attribute-def:sn" id="sn"/>
		<Attribute name="urn:mace:dir:attribute-def:givenName" id="givenName"/>
		<Attribute name="urn:mace:dir:attribute-def:displayName" id="displayName"/>
		<Attribute name="urn:mace:dir:attribute-def:mail" id="mail"/>
		<Attribute name="urn:mace:dir:attribute-def:telephoneNumber" id="telephoneNumber"/>
		<Attribute name="urn:mace:dir:attribute-def:title" id="title"/>
		<Attribute name="urn:mace:dir:attribute-def:initials" id="initials"/>
		<Attribute name="urn:mace:dir:attribute-def:description" id="description"/>
		<Attribute name="urn:mace:dir:attribute-def:carLicense" id="carLicense"/>
		<Attribute name="urn:mace:dir:attribute-def:departmentNumber" id="departmentNumber"/>
		<Attribute name="urn:mace:dir:attribute-def:employeeNumber" id="employeeNumber"/>
		<Attribute name="urn:mace:dir:attribute-def:employeeType" id="employeeType"/>
		<Attribute name="urn:mace:dir:attribute-def:preferredLanguage" id="preferredLanguage"/>
		<Attribute name="urn:mace:dir:attribute-def:manager" id="manager"/>
		<Attribute name="urn:mace:dir:attribute-def:seeAlso" id="seeAlso"/>
		<Attribute name="urn:mace:dir:attribute-def:facsimileTelephoneNumber" id="facsimileTelephoneNumber"/>
		<Attribute name="urn:mace:dir:attribute-def:street" id="street"/>
		<Attribute name="urn:mace:dir:attribute-def:postOfficeBox" id="postOfficeBox"/>
		<Attribute name="urn:mace:dir:attribute-def:postalCode" id="postalCode"/>
		<Attribute name="urn:mace:dir:attribute-def:st" id="st"/>
		<Attribute name="urn:mace:dir:attribute-def:l" id="l"/>
		<Attribute name="urn:mace:dir:attribute-def:o" id="o"/>
		<Attribute name="urn:mace:dir:attribute-def:ou" id="ou"/>
		<Attribute name="urn:mace:dir:attribute-def:businessCategory" id="businessCategory"/>
		<Attribute name="urn:mace:dir:attribute-def:physicalDeliveryOfficeName" id="physicalDeliveryOfficeName"/>

		<Attribute name="urn:oid:2.5.4.3" id="cn"/>
		<Attribute name="urn:oid:2.5.4.4" id="sn"/>
		<Attribute name="urn:oid:2.5.4.42" id="givenName"/>
		<Attribute name="urn:oid:2.16.840.1.113730.3.1.241" id="displayName"/>
		<Attribute name="urn:oid:0.9.2342.19200300.100.1.3" id="mail"/>
		<Attribute name="urn:oid:2.5.4.20" id="telephoneNumber"/>
		<Attribute name="urn:oid:2.5.4.12" id="title"/>
		<Attribute name="urn:oid:2.5.4.43" id="initials"/>
		<Attribute name="urn:oid:2.5.4.13" id="description"/>
		<Attribute name="urn:oid:2.16.840.1.113730.3.1.1" id="carLicense"/>
		<Attribute name="urn:oid:2.16.840.1.113730.3.1.2" id="departmentNumber"/>
		<Attribute name="urn:oid:2.16.840.1.113730.3.1.3" id="employeeNumber"/>
		<Attribute name="urn:oid:2.16.840.1.113730.3.1.4" id="employeeType"/>
		<Attribute name="urn:oid:2.16.840.1.113730.3.1.39" id="preferredLanguage"/>
		<Attribute name="urn:oid:0.9.2342.19200300.100.1.10" id="manager"/>
		<Attribute name="urn:oid:2.5.4.34" id="seeAlso"/>
		<Attribute name="urn:oid:2.5.4.23" id="facsimileTelephoneNumber"/>
		<Attribute name="urn:oid:2.5.4.9" id="street"/>
		<Attribute name="urn:oid:2.5.4.18" id="postOfficeBox"/>
		<Attribute name="urn:oid:2.5.4.17" id="postalCode"/>
		<Attribute name="urn:oid:2.5.4.8" id="st"/>
		<Attribute name="urn:oid:2.5.4.7" id="l"/>
		<Attribute name="urn:oid:2.5.4.10" id="o"/>
		<Attribute name="urn:oid:2.5.4.11" id="ou"/>
		<Attribute name="urn:oid:2.5.4.15" id="businessCategory"/>
		<Attribute name="urn:oid:2.5.4.19" id="physicalDeliveryOfficeName"/>




Copy your certificate and key into ``/etc/shibboleth`` with name ``sp-cert.pem`` and 
``sp-key.pem`` respectively.

::

    # service shibd restart
    # a2enmod ssl


**Create the Apache configuration**

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



The **plugin to enable for Shibboleth** is called external_authentication_sso.py and can be found under /opt/invenio/lib/python/invenio in the system-wide installation.


Added the file ``external_authentication_sso_scigaia.py``

in ``/opt/invenio/lib/python/invenio`` 

:download:`external_authentication_sso_scigaia.py <figures/external_authentication_sso_scigaia.py>`.


In a similar way the file ``access_control_config.py`` need to be adapted in order to enable the above mentioned plugin.

 
::
		sudo vim /opt/invenio/lib/python/invenio/access_control_config.py 
 
	 
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
				CFG_EXTERNAL_AUTHENTICATION = {
				CFG_EXTERNAL_AUTH_USING_SSO : ea_sso.ExternalAuthSSOSCIGAIA(True),
					"Local": None
				#    "Robot": ExternalAuthRobot(enforce_external_nicknames=True, use_zlib=False),
				#    "ZRobot": ExternalAuthRobot(enforce_external_nicknames=True, use_zlib=True)
				}

	   

Modified the file ```/opt/invenio/lib/python/invenio/webuser.py``

Added new method 

::


		def get_mail_from_mail_group(mailgroup):
		"""Return the first registered mail from colon or semicolon
		   group of email. Return the mailgroup when the email does not exists."""
		try:
			for mail in re.split(";|,",mailgroup):
				res = run_sql("SELECT email FROM user WHERE email LIKE %s", ("%"+mail+"%",))
				if res:
					return res[0][0]
		except OperationalError:
			register_exception()

		return mailgroup




Restart apache2 

::

		# service apache2 restart



Publish the metadata of your SP in a Federation.

For GrIDP contacts are avaible in `this page <http://gridp.garr.it/contacts.html>`_


