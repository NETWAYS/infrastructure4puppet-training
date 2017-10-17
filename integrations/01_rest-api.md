!SLIDE subsection
# ~~~SECTION:MAJOR~~~ Integrations


!SLIDE smbullets small

# Componets Rest APIs

Interact with component services.

* Retrieve infrastructure data from PuppetDB.
* ??? Foreman ??? Query the Node Classifier for classification information.
* ??? Foreman ??? Add rules to the Node Classifier during the provisioning process.
* Sign certificates for newly provisioned nodes.
* Clear environment caches on compile masters when deploying codebase.
* ...


!SLIDE smbullets small

# REST API Methods

Network based service interactions:

    @@@ Code
    https://www.example.org/api/v1/users/sheldon.cooper
    ^------Location-------^ ^-Endpoint-^ ^---Entity---^

* URI encodes network location, endpoint, and entity or collection.
* HTTP method describes the action to perform on the entity.

* Methods:
 * POST: Create a new entity.
 * GET: Retrieve a representation of an entity or collection.
 * PUT: Update an entity by replacing it with new data.
 * DELETE: Delete an entity or collection.


!SLIDE smbullets small

# Using the REST API

Setting up the curl connection:

    @@@ Sh
    $ curl --cert /etc/puppetlabs/puppet/ssl/certs/certname.pem \
        --key /etc/puppetlabs/puppet/ssl/private_keys/certname.pem \
        --cacert /etc/puppetlabs/puppet/ssl/ca/ca_crt.pem \
        -H 'Accept: yaml' https://master:8140/puppet/v3/node \
        /yourname.localdomain?environment=production

* Can reuse agent certificates.
* Can accept yaml or json as return types.
* Default method to GET request.
* Make PUT request and send data:
 * -X PUT --data '{"desired_state":"revoke"}' -H "Content-Type: text/pson"
* Make a DELETE request:
 * -X DELETE

