!SLIDE smbullets
# Lab ~~~SECTION:MAJOR~~~.~~~SECTION:MINOR~~~: Puppet Master Installation

* Objective:
 * Install und configure your Puppet Master.
* Steps:
 * Import and start with the trainings VM.
 * Add your virtual machine to the classroom infrastructure.
 * Do puppet runs against other masters.


!SLIDE supplemental exercises
# Lab ~~~SECTION:MAJOR~~~.~~~SECTION:MINOR~~~: Puppet Master Installation

## Objective:

****

* Install und configure your Puppet Master.

## Steps:

****

* Import and start with the trainings VM.
* Add your virtual machine to the classroom infrastructure.
* Do puppet runs against other masters.


!SLIDE supplemental solutions
# Lab ~~~SECTION:MAJOR~~~.~~~SECTION:MINOR~~~: Puppet Master Installation

****

##  Install und configure your Puppet Master

****

Import the VirtualBox Appliances:

Do not forget to set a new MAC address for this machine.

Start the VM and login:

Change the hostname to yourname:

Add the puppetlabs repository:

    @@@ Sh
    $ rpm --import https://yum.puppetlabs.com/RPM-GPG-KEY-puppetlabs
    $ yum install -y https://yum.puppetlabs.com/puppetlabs-release-pc1-el-7.noarch.rpm
    $ yum makecache

Install puppet agent and server:

    @@@ Sh
    $ yum install -y puppet-agent puppetserver

Turn off SE Linux and the firewall:

    @@@ Sh
    $ setenforce permissive
    $ vim /etc/sysconfig/selinux
    SELINUX=permissive

    $ systemctl disalbe firewalld; systemctl stop firewalld

On the master disable to act as a CA:

    @@@ Sh
    $ vim /etc/puppetlabs/puppetserver/bootstrap.cfg
    ...
    # To enable the CA service, leave the following line uncommented
    #puppetlabs.services.ca.certificate-authority-service/certificate-authority-service
    # To disable the CA service, comment out the above line and uncomment the line below
    puppetlabs.services.ca.certificate-authority-disabled-service/certificate-authority-disabled-service

    $ vim /etc/puppetlabs/puppetserver/conf.d/webserver.conf
    webserver: {
      access-log-config: /etc/puppetlabs/puppetserver/request-logging.xml
      client-auth: want
      ssl-host: 0.0.0.0
      ssl-port: 8140

      ssl-cert: /etc/puppetlabs/puppet/ssl/certs/yourname.localdomain.pem
      ssl-key: /etc/puppetlabs/puppet/ssl/private_keys/yourname.localdomain.pem
      ssl-ca-cert: /etc/puppetlabs/puppet/ssl/certs/ca.pem
    }

    Note: The key, certificate and ca root certificate files don't exist at the moment. You'll get the full paths by execute the following commands:

    @@@ Sh
    $ puppet master --configprint hostcert
    /etc/puppetlabs/puppet/ssl/certs/yourname.localdomain.pem
    $ puppet master --configprint hostprivkey
    /etc/puppetlabs/puppet/ssl/private_keys/yourname.localdomain.pem
    $ puppet master --configprint localcacert
    /etc/puppetlabs/puppet/ssl/certs/ca.pem

Ask your trainer for the ip address of the classroom master and add this host to the local hosts file:

    @@@ Sh
    $ vim /etc/hosts
    ...
    master-ip     overloard.localdomain overlord

Start an agent run to create a key and send a certificate request:

    @@@ Sh
    $ puppet agent -t --server=overlord.localdomain

    Waiting for the trainer signs your request and do another agent run.

Configure the agent on your master to runs against itself:

    @@@ Sh
    $vim /etc/puppetlabs/puppet/puppet.conf
    [main]
    server    = yourname.localdomain
    ca_server = overlord.localdomain

Finally start the puppetserver and do puppet runs against other masters:

    @@@ Sh
    $ systemctl enable puppetserver; systemctl start puppetserver
