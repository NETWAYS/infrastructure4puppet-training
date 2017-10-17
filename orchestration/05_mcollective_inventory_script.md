!SLIDE smbullets
# Lab ~~~SECTION:MAJOR~~~.~~~SECTION:MINOR~~~: Create an inventory script

* Objective:
 * Create a script for MCollective to create inventory reports
 * Run MCollective with your script


!SLIDE supplemental exercises
# Lab ~~~SECTION:MAJOR~~~.~~~SECTION:MINOR~~~: Create an inventory script

## Objective:

****

* Create a script for MCollective to create inventory reports
* Run MCollective with your script

## Steps:

****

* Create the script
* Run MCollective with this script


!SLIDE supplemental solutions
# Lab ~~~SECTION:MAJOR~~~.~~~SECTION:MINOR~~~: Create an inventory script

****

## Create the script

****

Create the file `simple_inventory.mc`

    @@@
    inventory do
      format "%25s: %10s [ %s / %s ]"
        fields { [
          identity,
          facts["operatingsystem"],
          facts["memoryfree"],
          facts["memorysize"]
        ] }
    end

****

## Run MCollective with this script

****

    @@@ Sh
    $ mco inventory --script simple_inventory.mc

If your nodes miss facts in the output run the following:

    @@@ Sh
    $ /opt/puppetlabs/bin/facter --puppet --yaml > /etc/puppetlabs/mcollective/facts.yaml
