Brekka multi-instance Tomcat DEB
================================

Overview
--------

Repackages a standard Apache Tomcat 7 zip distribution into a DEB archive for 
easy installation on Debian based Linux distributions. Also includes a set of 
scripts for creating and deleting independent Tomcat instances (with their own
catalina.base, running under their own user account). By default the 
installation does not create any instances, it is left to the user to create 
the first and any subsequent instances.

Note that the resulting deb archive excludes the following from the standard 
zip:

 * All of the webapps
 * Any *.bat files
 
The following additional artifacts will be included:

 * /etc/default/tomcat7   - Default configuration for all Tomcat instances.
 * /usr/bin/tomcat7       - User script for starting/stopping the Tomcat 
                            instance.
 * /usr/sbin/addtomcat7   - Creates a new instance.
 * /usr/sbin/deltomcat7   - Deletes a previously created instance.
 * /usr/sbin/tomcat7-init - Init script used to control the instances.


Requirements
------------

Java 7 or higher must be installed. Tested with OpenJDK 7.


Releases
--------

The latest release is 7.0.52, and can be found here: [tomcat-deb-7.0.52.deb](https://brekka.org/maven/content/repositories/releases/org/brekka/tomcat-deb/7.0.52/tomcat-deb-7.0.52.deb)


Licence
-------

The Apache Software License, Version 2.0 - http://www.apache.org/licenses/LICENSE-2.0.html


Building
--------

The Apache Tomcat zip must be downloaded manually and placed in the local Maven 
repository under the following path:

    ${user}/.m2/repository/org/apache/apache-tomcat/[version]/apache-tomcat-[version].zip

Once that is in place, the project can be built using Maven 3:

    mvn clean install
    

Usage
-----

For these examples we use 'tomcat7' as the name of an instance, but this can be
tailored to the application it will be hosting. The name of the instance will
be used for the service name, run-as user and directory under '/var/lib' so it
does need to be reasonably unique.

##### Install using dpkg:

    dpkg -i tomcat_7.0.52_all.deb
    

##### Create a new instance:

    addtomcat7 tomcat7
    
This will create a new Tomcat 7 instance under "/var/lib/tomcat7" and will
run as a service under the user "tomcat7". Not that the "server.xml" must
be amended prior to starting the instance if more than one instance is to
be deployed. This addtomcat7 script makes no attempt to assign unique 
ports.


##### Delete an instance:

    deltomcat7 tomcat7
    
The user will be prompted to confirm deletion of the instance. This will 
completely remove the instance include the configuration, logging and home
directory for the instance user.


##### Start/Stop/Restart an instance:

    service tomcat7 start/stop/restart

As root, the instance can be controlled like a regular service. If running
as the instance user itself, then the 'tomcat7' script can be used to
control the instance:
    
    tomcat7 start/stop/restart
    

