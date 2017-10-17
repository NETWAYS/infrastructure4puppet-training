!SLIDE smaller

# `mco` Syntax

    @@@Sh
    mco [application] [command] [arguments] [options]
    
* Application
 * The name of the application you want to start
* Command
 * Application command to run
 * Often executes actions on agents
* Arguments
 * Command arguments
 * Passed to actions on agents
* Options
 * `mco` options
 * often filters

!SLIDE smbullets

# `mco` help

* `mco help [agent]`
* `mco [application] --help`

!SLIDE smaller

# See all connected hosts

    @@@ Sh
    $ mco ping
    overlord.localdomain                     time=71.35 ms
    spoke01.localdomain                      time=111.82 ms
    
    
    ---- ping statistics ----
    2 replies max: 111.82 min: 71.35 avg: 91.59
    
!SLIDE smbullets

# Use facts for filtering

* MCollective can use facts for filtering
* Per default MCollective reads fact from a YAML file
 * Fill the YAML file with facts
 * use the `facter` plugin
 
~~~SECTION:handouts~~~

Using facter can be errournous due to the slow execution of facter.

An example how to create the facts YAML file can be found at https://docs.puppet.com/mcollective/plugin_directory/facter_via_yaml.html

If you still want to use facter: https://github.com/puppetlabs/mcollective-facter-facts

~~~ENDSECTION~~~

~~~SECTION:notes~~~

Using facter can be errournous due to the slow execution of facter.

An example how to create the facts YAML file can be found at https://docs.puppet.com/mcollective/plugin_directory/facter_via_yaml.html

If you still want to use facter: https://github.com/puppetlabs/mcollective-facter-facts

~~~ENDSECTION~~~

!SLIDE smaller

# Use facts for filtering

    @@@ Sh
    $ mco ping --with-fact osfamily=RedHat
    spoke01.localdomain                      time=60.12 ms
    overlord.localdomain                     time=100.36 ms
    
    
    ---- ping statistics ----
    2 replies max: 100.36 min: 60.12 avg: 80.24
    

!SLIDE smbullets

# Use plugins

* Packaged for supported operating systems
* installable as ruby files into directory structure

!SLIDE smaller

# Use a MCollective plugin to find vulnerable openssl versions

    @@@ Sh
    $ mco rpc package status package=openssl
    overlord.localdomain                     
        Arch: x86_64
      Ensure: 1:1.0.1e-42.el7.9
       Epoch: 1
        Name: openssl
      Output: nil
    Provider: yum
     Release: 42.el7.9
     Version: 1.0.1e
     
    spoke01.localdomain                      
       Arch: x86_64
    ...
    Summary of Ensure:
    
    1:1.0.1e-42.el7.9 = 2

!SLIDE smaller

# Use MCollective to install software

    @@@ Sh
    $ mco rpc package install package="lynx"
    
It is still better to use Puppet to install software

~~~SECTION:handouts~~~

Keep in mind you can combine all MCollective commands with the filters mentioned before.

~~~ENDSECTION~~~

~~~SECTION:notes~~~

Keep in mind you can combine all MCollective commands with the filters mentioned before.

~~~ENDSECTION~~~

!SLIDE smaller

# Use MCollective to update software (in batches)

    @@@ Sh
    $ mco rpc package update package=ca-certificates --batch=5 --batch-sleep=60
    
# Use MCollective to manage Puppet

Trigger Puppet runs via MCollective

    @@@ Sh
    $ mco puppet runall 1

The `1` indicates that one node after the other should run. Increase for concurrent runs.

~~~SECTION:handouts~~~

You will need the following packages for this to work:

* mcollective-puppet-agent (on all hosts)
* mcollective-puppet-client (on all hosts)
* mcollective-puppet-common (on the host you run the command)

~~~ENDSECTION~~~

~~~SECTION:notes~~~

You will need the following packages for this to work:

* mcollective-puppet-agent (on all hosts)
* mcollective-puppet-client (on all hosts)
* mcollective-puppet-common (on the host you run the command)

~~~ENDSECTION~~~

!SLIDE smaller

# Use MCollective to manage services

    @@@ Sh
    $ mco rpc service status service=puppet
    
Use Puppet for permanent changes and MCollective for short term changes in test environments, during outages, etc.

~~~SECTION:handouts~~~

You will need the following packages for this to work:

* mcollective-service-agent (on all hosts)
* mcollective-service-client (on all hosts)
* mcollective-service-common (on the host you run the command)

~~~ENDSECTION~~~


~~~SECTION:notes~~~

You will need the following packages for this to work:

* mcollective-service-agent (on all hosts)
* mcollective-service-client (on all hosts)
* mcollective-service-common (on the host you run the command)

~~~ENDSECTION~~~
