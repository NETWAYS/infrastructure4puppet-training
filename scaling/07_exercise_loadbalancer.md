!SLIDE smbullets
# Lab ~~~SECTION:MAJOR~~~.~~~SECTION:MINOR~~~: Load Balance Puppet

* Objective:
 * Configure your Puppet Master to work with the classroom proxy.
* Steps:
 * Add your master it to the classroom load balancer pool.
 * Run your Agent node(s) against the classroom proxy.


!SLIDE supplemental exercises
# Lab ~~~SECTION:MAJOR~~~.~~~SECTION:MINOR~~~: Load Balance Puppet

## Objective:

****

* Configure your Puppet Master to work with the classroom proxy.

## Steps:

****

* Add your master it to the classroom load balancer pool.
* Run your Agent node(s) against the classroom proxy.


!SLIDE supplemental solutions
# Lab ~~~SECTION:MAJOR~~~.~~~SECTION:MINOR~~~: Load Balance Puppet

****

## Configure your Puppet Master to work with the classroom proxy.

****

First set dns_alt_names in puppet.conf on your master:

    @@@ Sh
    $ vim /etc/puppetlabs/puppet/puppet.conf
    [main]
      ...
      dns_alt_names = proxy.localdomain
      ...

Remove your key and certificate:

    @@@ Sh
    $ rm -rf /etc/puppetlabs/puppet/ssl/*

Ask your trainer to clean your old certificate from CA.

Start a puppet run to get a new certificate with the alternative name proxy.localdomain:

    @@@ Sh
    $ puppet agent -t

Waiting for a new signed certificate sign by the trainer.

Add a new resource to the site.pp on your master:

If you hadn't add module haproxy to Puppetfile, do it now and don't forgett the modules haproxy depends on:

    @@@ Sh
    $ cd ~/puppetcode
    $ git pull
    $ vim manifests/site.pp
    node "yourname.localdomain" {
      @@haproxy::balancermember { $::hostname:
        listening_service => 'puppet00',
        server_names      => $::fqdn,
        ipaddresses       => $::ipaddress,
        ports             => '8140',
        options           => 'check',
      }
    }

    $ git add manifests/site.pp
    $ git commit -m 'add balancermember to node yourname'
    $ git push origin production

Configure your agent to runs against the proxy:

    @@@
    $ vim /etc/puppetlabs/puppet/puppet.conf
    [main]
      ...
      server = proxy.localdomain
      ...

Finally start an agent run several times to configure and test the load balancer setup:

    @@@ Sh
    $ puppet agent -t
