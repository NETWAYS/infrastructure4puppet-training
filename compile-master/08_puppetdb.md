!SLIDE smbullets small

# PuppetDB

PuppetDB stores:

* The most recent facts from every node.
* The most recent catalog for every node.
* Necessary for "stored configs" feature, required for exported resources.
* Optionally, 14 days (configurable) of event reports for every node.

Every Compile Master has to send the information to the PuppetDB.

Test if node info are available on a master with:

    @@@ Sh
    $ puppet node status yourname-agent.localdomain


!SLIDE smbullets
# Lab ~~~SECTION:MAJOR~~~.~~~SECTION:MINOR~~~: PuppetDB Terminus

* Objective:
 * Configure on your master the connection to the classroom PuppetDB.
* Steps:
 * Install the plugin binaries.
 * Configure the plugin.
 * Check the logs after a succesful puppet run.


!SLIDE supplemental exercises
# Lab ~~~SECTION:MAJOR~~~.~~~SECTION:MINOR~~~: PuppetDB Terminus

## Objective:

****

* Configure on your master the connection to the classroom PuppetDB.

## Steps:

****

* Install the plugin binaries.
* Configure the plugin.
* Check the logs after a succesful puppet run.


!SLIDE supplemental solutions
# Lab ~~~SECTION:MAJOR~~~.~~~SECTION:MINOR~~~: PuppetDB Terminus

****

## Configure on your master the connection to the classroom PuppetDB.

****

Install the RPM for the plugin:

    @@@ Sh
    $ yum install -y puppetdb-termini

Configure the plugin:

    @@@ Sh
    $ vim /etc/puppetlabs/puppet/puppetdb.conf
    [main]
    server_urls = https://overlord.localdomain:8081

    $ puppet master --configprint route_file
    /etc/puppetlabs/puppet/routes.yaml
    $ vim /etc/puppetlabs/puppet/routes.yaml
    ---
    master:
      facts:
        terminus: puppetdb
        cache: yaml

    $ vim /etc/puppetlabs/puppet/puppet.conf
    [master]
      storeconfigs = true
      storeconfigs_backend = puppetdb

Restart your Puppet Server:

    @@@ Sh
    $ systemctl restart puppetserver

Do a puppet run on your Agent VM:

    @@@ Sh
    $ puppet agent -t

Check the logs after a succesful puppet run:

    @@@ Sh
    $ tail -f /var/log/puppetlabs/puppetserver/puppetserver.log
    ... INFO  [p.p.command] [...] [replace facts] yourname-agent.localdomain
    ... INFO  [p.p.command] [...] [replace catalog] yourname-agent.localdomain
    ... INFO  [p.p.command] [...] [store report] puppet v4.x.x - yourname-agent.localdomain
