!SLIDE smbullets small

# Basic Tuning Parameters

* Configure JVM for stability in certain environments.
 * Increase heap size for capacity under heavy load.
 * Decrease heap size when running out of system memory.

* Default heap size of different commponents should be:
 * Puppet Master: 2 GB
 * PuppetDB: 256 MB
 * ActiveMQ: 512 MB

Settings have to be changed thru Java Options Xms and Xmx, i.e. for the puppetserver:

    @@@ Sh
    $ vim /etc/sysconfig/puppetserver
    JAVA_ARGS="-Xms2G -Xmx2G -XX:MaxPermSize=256m"


~~~SECTION:handouts~~~

Notes:

* Capacity problems are often indicated with JVM application logs.
* Running out of system memory is often indicated by kernel logs.

~~~ENDSECTION~~~
