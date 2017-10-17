!SLIDE smbullets small

# Report Processors

* Ruby code
* Connect to (third party) tools that store reports


!SLIDE smbullets small

# Third Party Integration

Example: Reporting to Logstash.

* Use Puppet module to install report processors
* Connects directly to Logstash
* Use Elastic Stack to store reports

~~~SECTION:notes~~~

Elastic stack is already installed on overlord. Can be used by students to view their own reports.

The module seems not to work as expected. You will need workarounds. See the following lab for working code.

~~~ENDSECTION~~~

~~~SECTION:handouts~~~

The module seems not to work as expected. You will need workarounds. See the following lab for working code.

~~~ENDSECTION~~~

!SLIDE smbullets

# Reporting to Elastic Stack

You will need a working Elastic Stack for this.

* Logstash (collect, transform and transport reports)
* Elasticsearch (store reports)
* Kibana (view reports, create graphs, etc.)

~~~SECTION:handouts~~~

If you want to learn how to deploy and use Elastic Stack, see: https://www.netways.de/en/events_trainings/elastickstack/

An example Logstash configuration for usage with the Puppet Logstash Reporter

    @@@
    input {
      tcp {
        port => 5999
        type => "puppet-logstash-reporter"
        codec => "json"
      }
    }
    output {
      elasticsearch {
      }
    }

~~~ENDSECTION~~~

~~~SECTION:notes~~~

If you want to learn how to deploy and use Elastic Stack, see: https://www.netways.de/en/events_trainings/elastickstack/

~~~ENDSECTION~~~

!SLIDE smbullets
# Lab ~~~SECTION:MAJOR~~~.~~~SECTION:MINOR~~~: Add the Logstash Reporter

* Objective:
 * Send your Puppet reports to Logstash


!SLIDE supplemental exercises
# Lab ~~~SECTION:MAJOR~~~.~~~SECTION:MINOR~~~: Add the Logstash Reporter

## Objective:

****

* Send your Puppet reports to Logstash

## Steps:

****

* Install the `elasticsearch-logstash_reporter` module via r10k
* Add the `logstash_reporter` class to your puppetserver
* Use a workaround to deploy the report processor
* Review your reports in Kibana


!SLIDE supplemental solutions
# Lab ~~~SECTION:MAJOR~~~.~~~SECTION:MINOR~~~: Add the Logstash Reporter

****

## Send your Puppet reports to Logstash

****

Install the `elasticsearch-logstash_reporter` module via r10k

    @@@ Sh
    $ echo 'mod "elasticsearch-logstash_reporter"' >> Puppetfile

Add the logstash_reporter class to your site.pp

    @@@
    class { 'logstash_reporter':
      logstash_host => 'overlord.localdomain',
      logstash_port => 5999,
    }
    
Push your changes to the puppetserver

    $ git commit -am "Add Logstash reporter"
    $ git push

Create a `puppet` user on your agent

    @@@ Sh
    $ useradd -r puppet
    
(This is a dependency of the beforementioned Puppet module). The puppetserver already has this user

Add `logstash` as report to your `puppet.conf`

    @@@ Sh
    $ vim /etc/puppetlabs/puppet/puppet.conf
    ...
    reports = foreman,logstash
    ...
    systemctl restart puppetserver

Run puppet to apply your changes

    @@@ Sh
    $ puppet agent -t

Run Puppet again to generate a report

    @@@ Sh
    $ puppet agent -t
    
Review your report in Kibana

    @@@
    http://overlord:5601


!SLIDE smbullets small

