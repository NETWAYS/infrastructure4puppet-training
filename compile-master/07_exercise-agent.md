!SLIDE smbullets
# Lab ~~~SECTION:MAJOR~~~.~~~SECTION:MINOR~~~: Agent Installation

* Objective:
 * Install und configure your Agent VM.
* Steps:
 * Import and Start with a second trainings VM.
 * Add your virtual machine to the classroom infrastructure.
 * Do a puppet run against your masters.


!SLIDE supplemental exercises
# Lab ~~~SECTION:MAJOR~~~.~~~SECTION:MINOR~~~:  Agent Installation

## Objective:

****

* Install und configure your Agent VM.

## Steps:

****

* Import and Start with a second trainings VM.
* Add your virtual machine to the classroom infrastructure.
* Do a puppet run against your masters.

!SLIDE supplemental solutions
# Lab ~~~SECTION:MAJOR~~~.~~~SECTION:MINOR~~~: Agent Installation

****

##  Install und configure your Agent VM

****

Import the VirtualBox Appliances:

Do not forget to set a new MAC address for this machine.

Start the VM and login:

Change the hostname to yourname-agent:

Add the puppetlabs repository:

    @@@ Sh
    $ rpm --import https://yum.puppetlabs.com/RPM-GPG-KEY-puppetlabs
    $ yum install -y https://yum.puppetlabs.com/puppetlabs-release-pc1-el-7.noarch.rpm
    $ yum makecache

Install puppet agent:

    @@@ Sh
    $ yum install -y puppet-agent

Turn off SE Linux and the firewall:

    @@@ Sh
    $ setenforce permissive
    $ vim /etc/sysconfig/selinux
    SELINUX=permissive

    $ systemctl disalbe firewalld; systemctl stop firewalld

Configure the agent to use your puppet master but the CA on the classroom master:

    @@@ Sh
    $ vim /etc/puppetlabs/puppet/puppet.conf
    [main]
      server=yourname.localdomain
      ca_server=overlord.localdomain

Create host entries in the local hosts file:

    @@@ Sh
    $ vim /etc/hosts
    ...
    master-ip     overlord.localdomain overlord
    yourname-ip   yourname.localdomain yourname

Do an agent run to request for a certificate:

    @@@ Sh
    $ puppet agent -t

Wait for the trainer to sign your request and do a run again.

