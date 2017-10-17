!SLIDE smbullets
# Lab ~~~SECTION:MAJOR~~~.~~~SECTION:MINOR~~~: Connect your MCollective to the classroom

* Objective:
 * Connect your MCollective servers to the central ActiveMQ infrastructure
 * Run MCollective `ping` to see all connected nodes


!SLIDE supplemental exercises
# Lab ~~~SECTION:MAJOR~~~.~~~SECTION:MINOR~~~: Connect your MCollective to the classroom

## Objective:

****

* Connect your MCollective servers to the central ActiveMQ infrastructure
* Run MCollective `ping` to see all connected nodes

## Steps:

****

* Add the ActiveMQ node to your `/etc/hosts` file
* Add the ActiveMQ node to your MCollective Configuration
* Start MCollective
* Run the `mco` command


!SLIDE supplemental solutions
# Lab ~~~SECTION:MAJOR~~~.~~~SECTION:MINOR~~~: Connect your MCollective to the classroom

****

## Add the ActiveMQ node to your `/etc/hosts` file

****

    @@@ Sh
    vim /etc/hosts
    192.168.23.191  spoke01.localdomain
    
Note that this IP Address has to be replaced to fit to your training environment

****

## Add the ActiveAdd the ActiveMQ node to your MCollective ConfigurationMQ node to your MCollective Configuration

****

For the client

    @@@ Sh
    $ vim /etc/puppetlabs/mcollective/client.cfg
    ...
    plugin.activemq.pool.1.host = spoke01.localdomain
    ...
    
For the server 

    @@@ Sh
    $ vim /etc/puppetlabs/mcollective/server.cfg
    ...
    plugin.activemq.pool.1.host = spoke01.localdomain
    ...
    
Please keep in mind that this classroom setup uses default passwords. In a production environment you should use other methods of authenticating against ActiveMQ.

****

## Start MCollective

****

    @@@ Sh
    $ systemctl status mcollective
    
****

## Run `mco` with a simple query

****

    @@@ Sh
    $ mco ping
    
Now run the same command just on your training VMs

    @@@ Sh
    $ mco ping -I /yourname/
    
Keep in mind that you shoud use a filter like this whenever you are running actions on nodes to avoid interfering with your classmates.
