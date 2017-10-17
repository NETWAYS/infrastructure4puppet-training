!SLIDE smbullets small

# External Node Classifier

* Called by a Puppet Master.
* ENCs can co-exist with standard node definitions in `site.pp`.
* Examples:
 * The Foreman
 * Puppet Enterprise Console
* Connecting an ENC:

<pre>
[master]
  node_terminus = exec
  external_nodes = /usr/local/bin/puppet_node_classifier
</pre>


!SLIDE smbullets
# Lab ~~~SECTION:MAJOR~~~.~~~SECTION:MINOR~~~: Using Foreman's ENC

* Objective:
 * Configure on your master the connection to Foreman's ENC.
* Steps:
 * Download, install and configure the node.rb script from the Foreman project.
 * Test the script.
 * Configure your Puppet Master to use the new ENC script.
 * Do a puppet run on your agent machine.


!SLIDE supplemental exercises
# Lab ~~~SECTION:MAJOR~~~.~~~SECTION:MINOR~~~: Using Foreman's ENC

## Objective:

****

* Configure on your master the connection to Foreman's ENC.

## Steps:

****

* Download, install and configure the node.rb script from the Foreman project.
* Test the script.
* Configure your Puppet Master to use the new ENC script.
* Do a puppet run on your agent machine.


!SLIDE supplemental solutions
# Lab ~~~SECTION:MAJOR~~~.~~~SECTION:MINOR~~~: Using Foreman's ENC

****

## Configure on your master the connection to Foreman's ENC.

****

Install the node.rb script:

    @@@ Sh
    $ curl -O https://raw.githubusercontent.com/theforeman/puppet-foreman/master/files/external_node_v2.rb
    $ install -o puppet -g puppet -m 755 external_node_v2.rb /etc/puppetlabs/puppet/node.rb

Change to ruby interpreter of Puppet AIO:

    @@@ Sh
    $ vim /etc/puppetlabs/puppet/node.rb
    #!/usr/bin/env /opt/puppetlabs/puppet/bin/ruby
    ...

Set configuration for node.rb:

    @@@ Sh
    $ puppet config print yamldir —section master
    /opt/puppetlabs/server/data/puppetserver
    $ vim /etc/puppetlabs/puppet/foreman.yaml
    ---
    :url: "https://overlord.localdomain"
    :ssl_ca: "/etc/puppetlabs/puppet/ssl/certs/ca.pem"
    :ssl_cert: "/etc/puppetlabs/puppet/ssl/certs/yourname.localdomain.pem"
    :ssl_key: "/etc/puppetlabs/puppet/ssl/private_keys/yourname.localdomain.pem"
    :user: ""
    :password: ""
    :puppetdir: "/opt/puppetlabs/server/data/puppetserver"
    :puppetuser: "puppet"
    :facts: true
    :timeout: 60
    :report_timeout: 60
    :threads: null

Ask the trainer to autorize your master to receive ENC information.

Configure your Puppet Master to use the new ENC script:

    @@@ Sh
    $ vim /etc/puppetlabs/puppet/puppet.conf
    [master]
      external_nodes = /etc/puppetlabs/puppet/node.rb
      node_terminus = exec

After restarting your Puppet Master, do a puppet run on your agent machine:

    @@@ Sh
    $ puppet agent -t

And control the log on your Puppet Master:

    @@@ Sh
    $ tail -f /var/log/puppetlabs/puppetserver/puppetserver.log
    ... INFO  [...] [puppetserver] Puppet Caching node for yourname-agent.localdomain
    ... INFO  [...] [puppetserver] Puppet 'replace_facts' command for yourname-agent.localdomain ...
    ... INFO  [...] [puppetserver] Puppet Caching node for yourname-agent.localdomain
    ... INFO  [...] [puppetserver] Puppet Compiled catalog for yourname-agent.localdomain ...
    ... INFO  [...] [puppetserver] Puppet Caching catalog for yourname-agent.localdomain
    ... INFO  [...] [puppetserver] Puppet 'replace_catalog' command for yourname-agent.localdomain ...

Run the node.rb script alone:

    @@@ Sh
    $ sudo -u puppet /etc/puppetlabs/puppet/node.rb yourname-agent.localdomain
    ---
    classes: {}
    parameters:
      puppetmaster: overlord.localdomain
      domainname: localdomain
      root_pw:
      puppet_ca: overlord.localdomain
    ...

