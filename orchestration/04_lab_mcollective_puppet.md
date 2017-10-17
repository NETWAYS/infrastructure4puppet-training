!SLIDE smbullets
# Lab ~~~SECTION:MAJOR~~~.~~~SECTION:MINOR~~~: Stop and start Puppet via MCollective

* Objective:
 * Stop Puppet via MCollective
 * Run Puppet once via MCollective
 * Start Puppet via MCollective


!SLIDE supplemental exercises
# Lab ~~~SECTION:MAJOR~~~.~~~SECTION:MINOR~~~: Stop and start Puppet via MCollective

## Objective:

****

* Stop Puppet via MCollective
* Run Puppet once via MCollective
* Start Puppet via MCollective

## Steps:

****

* Install the MCollective Plugins
* Stop Puppet via MCollective
* Run Puppet once via MCollective
* Start Puppet via MCollective


!SLIDE supplemental solutions
# Lab ~~~SECTION:MAJOR~~~.~~~SECTION:MINOR~~~: Stop and start Puppet via MCollective

****

## Install the MCollective Plugins

****

    @@@ Sh
    $ yum -y install 'mcollective-puppet*'
    $ systemctl restart mcollective


****

## Stop Puppet via MCollective

****


    @@@ Sh
    $ mco rpc service stop service=puppet

****

## Run Puppet once via MCollective

****

    @@@ Sh
    $ mco puppet runall 1 -I /yourname/
    
****

## Start Puppet via MCollective

****


    @@@ Sh
    $ mco rpc service start service=puppet

!SLIDE smbullets

# Use MCollective to create reports

* Use `.mc` scripts to format reports
* Uses facts from YAML file or `facter` 
