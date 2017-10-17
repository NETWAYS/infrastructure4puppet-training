!SLIDE smbullets small

# Puppet Master REST API

https://`master`:8140/puppet/v3/`endpoint`/`certname`?environment=`env`

Retrieve information with a GET request.

* Request a catalog:
 * GET /puppet/v3/catalog/`certname`?environment=`env`
* Request node information, including facts:
 * GET /puppet/v3/node/`certname`?environment=`env`
* Request master status:
 * GET /puppet/v3/status/`certname`?environment=production
* Authorization configured in `auth.conf`.


~~~SECTION:handouts~~~

Notes:

The status endpoint requires values for the name and the environment, simply because that's the required argument format. These values are discarded and not used for anything.

~~~ENDSECTION~~~


!SLIDE smbullets
# Lab ~~~SECTION:MAJOR~~~.~~~SECTION:MINOR~~~: Request a node with curl

* Objective:
 * Instructor will enable API access in auth.conf
 * Request node information from the Puppet Master using the command line binary.


!SLIDE supplemental exercises
# Lab ~~~SECTION:MAJOR~~~.~~~SECTION:MINOR~~~: Request a node with curl

## Objective:

****

* Request node information from the Puppet Master using the command line binary.

## Steps:

****

Not required.


!SLIDE supplemental solutions
# Lab ~~~SECTION:MAJOR~~~.~~~SECTION:MINOR~~~: Request a node with curl

****

## Use the certificate authority API to clean and re-sign agent certificates.

****

First save the authentication stuff in an environment varibale:

    @@@ Sh
    $ export CERT_OPTIONS=" \
        --cert /etc/puppetlabs/puppet/ssl/certs/yourname.localdomain.pem \
        --key /etc/puppetlabs/puppet/ssl/private_keys/yourname.localdomain.pem \
        --cacert /etc/puppetlabs/puppet/ssl/certs/ca.pem"

Request node information of your master:

    @@@ Sh
    $ curl -X GET ${CERT_OPTIONS} https://overlord.localdomain:8140 \
        /puppet/v3/node/yourname.localdomain?environment=production


!SLIDE smbullets small

# Clearing Environment Caches

Master caches codebase for compilation performance:

* Configured with environment_timeout.
* Expire all environments with:
 * DELETE https://`master`:8140/puppet-admin-api/v1/environment-cache
* Expire single named environment with:
 * DELETE https://`master`:8140/puppet-admin-api/v1/environment-cache?environmnet=`env`


~~~SECTION:handouts~~~

Notes:

When running multiple compile masters, the environment cache must be cleared on each master.

~~~ENDSECTION~~~
