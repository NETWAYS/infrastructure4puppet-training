!SLIDE smbullets small

# Foreman API

https://`foreman`/api/v2/`endpoint`

* JSON API
* Shared also with Katello
* Requires username and password
* Collections are paged
* Search strings like provided in the WebGUI


~~~SECTION:handouts~~~

Notes:

Reference at https://theforeman.org/api/1.12/index.html.

~~~ENDSECTION~~~


!SLIDE smbullets
# Lab ~~~SECTION:MAJOR~~~.~~~SECTION:MINOR~~~: Use the foreman API

* Objective:
 * Use the API to query node objects.
* Steps:
 * Query the information of your master node.
 * Query the just the status of your master.
 * Get all nodes with CentOS on it.


!SLIDE supplemental exercises
# Lab ~~~SECTION:MAJOR~~~.~~~SECTION:MINOR~~~: Use the foreman API

## Objective:

****

* Use the API to query node objects.

## Steps:

****

* Query the information of your master node.
* Query the just the status of your master.
* Get all nodes with CentOS on it.


!SLIDE supplemental solutions
# Lab ~~~SECTION:MAJOR~~~.~~~SECTION:MINOR~~~: Use the foreman API

****

## Use the API to query node objects.

****

Query the information of your master node:

    @@@ Sh
    $ curl --user admin:changeme -H "Content-Type:application/json" \
        -H "Accept:application/json" -k https://overlord.localdomain/ \
        api/v2/hosts/overlord.localdomain \
        | python -m json.tool

Query the just the status of your master:

    @@@ Sh
    $ curl --user admin:changeme -H "Content-Type:application/json" \
        -H "Accept:application/json" -k https://overlord.localdomain/ \
        api/v2/hosts/overlord.localdomain/status \
        | python -m json.tool

Get all nodes with CentOS on it:

    @@@ Sh
    $ curl --user admin:changeme -H "Content-Type:application/json" \
        -H "Accept:application/json" -k https://overlord.localdomain/ \
        api/v2/hosts?search=facts.osfamily%3DCentOS \
        | python -m json.tool
        
