Getting-started-with-Cloudant-NoSQL-Database
============================================

Getting started with Cloudant NoSQL Database


DISCLAIMER----This routine and info wasn´t created for me!. It´s available in https://www.ng.bluemix.net/docs/#services/Cloudant/index.html#Cloudant 
My purpose is to invite all github users to know IBM Bluemix Platform.

IBM® Cloudant NoSQL Database for Bluemix™ is a NoSQL database as a service (DBaaS) built from the ground up to scale globally, run non-stop, and handle a wide variety of data types like JSON, full-text, and geo spatial. Cloudant is an operational data store optimized to handle concurrent reads and writes, and provide high availability and data durability.

Get help by using the support portal in the Cloudant NoSQL Database dashboard or by contacting mailto://support@cloudant.com

Learn about using Cloudant NoSQL Database with live demos and code examples in our For Developers section, or get into the details with our API Documentation.
You can access the service using any one of the following methods:

    The Web Console
    Applications outside of Bluemix
    Bluemix applications

The Web Console

You can launch the management dashboard console from within IBM Bluemix.
Applications outside of Bluemix

The connection and credential information for the service instance is available in the Bluemix VCAP_SERVICES environment variable. It provides the following information to connect to the Cloudant NoSQL database:

    Host name
    Port number
    Username and password
    URL

Bluemix applications

You can use the VCAP_SERVICES environment variable to connect to the Cloudant NoSQL database. You can use the Bluemix user interface to create applications and services, or you can do the same from the command line interface by using the CloudFoundry cf tool.

Before we get started you need to install the CloudFoundry cf tool on your desktop. Instructions on how to do that can be found in the Bluemix Quick Start guide.

    Managing Cloudant NoSQL Database instance with CLI
    First, create a new service instance with the cf create-service command. Type the following command at terminal:

    $ cf create-service

    cf prompts you to choose one of the available service offerings, and then asks you for a unique instance name and the plan for the service. It will present you with a random name but you can change it if you want.
    After creating a new service, you can use the cf services command to list all available service instances that you have created.

    $ cf services

    Before using a service in an application, you must bind the service to your application. Use the cf bind-service command:

    $ cf bind-service

    You need to select one of the applications and one of the services from the list. Once the binding action succeeds, cf will return a message to you.
    Connecting to the service in your application
    After binding a service instance to the application, code similar to the example below is added to your VCAP_SERVICES environment variable.

    { "cloudantNoSQLDB": { "name": "Cloudant-3s", "label": "cloudantNoSQLDB", "plan": "shared", "credentials": { "username": "someusername", "password": "secret", "host": "myhost-bluemix.cloudant.com", "port": 443, "url": "https://someusername:secret@myhost-bluemix.cloudant.com" } } }

    As you can see, the VCAP_SERVICE environment variable includes the following items:

    key
        The name of the service (cloudantNoSQLDB)
    name
        The user provided name of the service instance
    host
        The host name of Cloudant NoSQLDB server
    port
        The port number of the Cloudant NoSQLDB server
    username
        The user name for authentication
    password
        The password for authentication
    url
        The url of the Cloudant NoSQLDB instance

    Now you can use this Cloudant NoSQLDB instance from a variety of Bluemix run-times like java, was-liberty, node.js application and ruby applications.
    Interacting with Cloudant NoSQLDB instance to insert and query data
        Cloudant Developer homepage
        Using Cloudant from Python
        Using Cloudant from Java
        Using Cloudant from Ruby
        Cloudant Sync for Android
        Cloudant Sync for iOS
    Pushing your application to Bluemix
    After finished the coding, you can deploy your application to Bluemix environment for verification. To deploy an application, you need to enter into the root directory of the Java application and use the following command:

    $ cf push

    Unbinding or deleting a service instance
    To unbind a service instance from an application, you can use the following command:

    cf unbind-service

    To delete a service instance use the following command:

    cf delete-service

Binding a Cloudant NoSQL Database to Liberty applications

If you push an application or a packaged Liberty server and bind to a NoSQL database, the Liberty buildpack automatically generates or updates CouchDB-related server.xml entries.
When you push an application, the Liberty buildpack generates a server.xml file. If you bind your application to a NoSQL database, the Liberty buildpack generates the associated server.xml stanzas that are required to access the database. Additionally, the Liberty buildpack provides the required JAR files of the client driver. In summary, the Liberty buildpack:

    Generates a <couchdb> stanza.
    Generates a <library> stanza with an embedded <fileset> stanza.
    Adds the couchdb-1.0 feature to the <featureManager> stanza.
    Adds the classloader for the CouchDB ektorp libraries to the <application> stanza.

The Liberty buildpack must generate a jndiName in the <couchdb> stanza. Because the Liberty buildpack does not have access to the JNDI name that is used by the application, it generates a JNDI name with the convention couchdb/<service_name> where the <service_name> is the name of the bound service.

For example, if you bind a Cloudant NoSQL Database that is named mycloudant to the application, the Liberty buildpack generates a JNDI name of couchdb/mycloudant. When you develop the application and create the Cloudant NoSQL Database, you must ensure that the JNDI name that is used by the application matches the JNDI name that is generated by the Liberty buildpack.
The following example shows the generated configuration when an application is pushed and bound to a Cloudant NoSQL Database named mycloudant.

<featureManager> <feature>webProfile-6.0</feature> <feature>jaxrs-1.1</feature> <feature>couchdb-1.0</feature> </featureManager> <application context-root='/' location='../../../../../' name='myapp' type='war'> <classloader commonLibraryRef='cloudantNoSQLDB-library'> </application> <couchdb id='cloudantNoSQLDB-mycloudant' jndiName='couchdb/mycloudant' libraryRef='cloudantNoSQLDB-library' password='${cloud.services.mycloudant.connection.password}' url='${cloud.services.mycloudant.connection.url}' username='${cloud.services.mycloudant.connection.username}'/> <library id='cloudantNoSQLDB-library'> <fileset dir='${server.config.dir}/lib' id='cloudantNoSQLDB-fileset' includes='commons-codec-1.6.jar commons-io-2.0.1.jar commons-logging-1.1.1.jar httpclient-4.2.5.jar httpclient-cache-4.2.5.jar httpcore-4.2.5.jar jackson-annotations-2.2.2.jar jackson-core-2.2.2.jar jackson-databind-2.2.2.jar jcl-over-slf4j-1.6.6.jar org.ektorp-1.4.1.jar slf4j-api-1.6.6.jar slf4j-jdk14-1.6.6.jar'/> </library>

Pushing a Packaged Server

When you push a packaged server, the Liberty buildpack detects and uses the server.xml file in the compressed file that is pushed. When you bind the packaged server to a NoSQL database, the Liberty buildpack checks the server.xml file for <couchdb> entries that correspond to the bound database.

If the Liberty buildpack does not find the <couchdb> stanza, it is created and added to the server.xml file.

If you provide configuration in your server.xml file, the configuration must be coherent. The <couchdb>, <library>, and <fileset> stanzas must be configured properly. The <featureManager> must contain the couchdb-1.0 feature, and the <application> or <webApplication> stanza must contain the classloader that includes the library. Additionally, your configuration stanzas must contain configuration ids as described in the next section.
The Liberty buildpack searches for existing configuration by using the configuration IDs in the stanzas. The following algorithms are used to generate the configuration IDs that are used in the search:

    The <couchdb> search uses a configuration ID of <service_type>-<service_name>.
    The <library> search uses a configuration ID of <service_type>-library.
    The <fileset> search uses a configuration ID of <service_type>-fileset.

The <service_type> is a lowercase string constant. For Cloudant NoSQL Database, the service_type is cloudantNoSQLDB. The <service_name> is the name of the service. For example, if you create a Cloudant NoSQL Database with the name "mycloudant", "mycloudant" is the service_name and the cloudant stanza ID is 'cloudantNoSQLDB-mycloudant'. If you provide configuration stanzas, your configuration IDs must be created with the necessary algorithms. Failure to comply with these conventions results in errors.

If the Liberty buildpack finds existing configuration, it checks and updates if necessary. For example, the buildpack might update the following stanzas and attributes: The user, password and url attributes in the <couchdb> stanza or the dir and includes attributes in the <fileset> stanza.
Note: In this case, the Liberty buildpack does not update the jndiName attribute.

You can provide your own client driver JAR files. The client driver JAR files must be placed in the usr/servers/<servername>/lib directory. If you do not provide client driver JAR files, the Liberty buildpack provides the files. Any client JAR files that you provide must use the standard names that are established by the providing vendor. You cannot rename client JAR files.
Note: To avoid classloading conflicts, you should not provide the client driver JAR files in the WEB-INF/lib directory of your application.

The following code is a sample of Cloudant NoSQL Database configuration that might be provided in the local server.xml file. This sample assumes that when the packaged server is pushed to the cloud, it is bound to a Cloudant NoSQL Database named "mycloudant". In this case, the jndiName attribute can be whatever value you prefer. The value of the jndiName attribute is independent of the service name.

<couchdb id="cloudantNoSQLDB-mycloudant" jndiName='couchdb/mycloudant' libraryRef='cloudantNoSQLDB-library' password='somepass' url='https://someuser:somepass@someuser.cloudant.com'/> <library id="cloudantNoSQLDB-library"> <fileset id='cloudantNoSQLDB-fileset' dir='c:/cloudantlibs' includes='commons-codec-1.6.jar commons-io-2.0.1.jar commons-logging-1.1.1.jar httpclient-4.2.5.jar httpclient-cache-4.2.5.jar httpcore-4.2.5.jar jackson-annotations-2.2.2.jar jackson-core-2.2.2.jar jackson-databind-2.2.2.jar jcl-over-slf4j-1.6.6.jar org.ektorp-1.4.1.jar slf4j-api-1.6.6.jar slf4j-jdk14-1.6.6.jar'/> </library>

When the server.xml file is pushed to the cloud, the Liberty buildpack updates the CouchDB configuration stanzas as follows:

<couchdb id='cloudantNoSQLDB-mycloudant' jndiName='couchdb/mycloudant' libraryRef='cloudantNoSQLDB-library' password='${cloud.services.mycloudant.connection.password}' url='${cloud.services.mycloudant.connection.url}' username='${cloud.services.mycloudant.connection.username}'/> <library id='cloudantNoSQLDB-library'> <fileset dir='${server.config.dir}/lib' id='cloudantNoSQLDB-fileset' includes='commons-codec-1.6.jar commons-io-2.0.1.jar commons-logging-1.1.1.jar httpclient-4.2.5.jar httpclient-cache-4.2.5.jar httpcore-4.2.5.jar jackson-annotations-2.2.2.jar jackson-core-2.2.2.jar jackson-databind-2.2.2.jar jcl-over-slf4j-1.6.6.jar org.ektorp-1.4.1.jar slf4j-api-1.6.6.jar slf4j-jdk14-1.6.6.jar'/> </library>

Sample for Cloudant on Liberty

The following developerWorks article contains a sample that shows how this auto-configuration works:

https://developer.ibm.com/bluemix/2014/07/08/cloudant_on_bluemix/
