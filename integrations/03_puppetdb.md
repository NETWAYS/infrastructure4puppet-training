!SLIDE smbullets small

# PuppetDB

Return resources matching the query string:

* Facts
* Resources
* Nodes
* Catalogs
* Metrics
* Reports


~~~SECTION:handouts~~~

Notes:

Documented at http://docs.puppetlabs.com/puppetdb/3.0/api/index.html

~~~ENDSECTION~~~


!SLIDE smbullets small

# PuppetDB

Query API:

https://`puppetdb`:8081/pdb/query/v4/`endpoint`

Query string is

* URL-encoded JSON array
* Multi-dimensional
* Describes a complex comparison operation in prefix notation.

Paged queries; e.g. "11-20 of 164"

* Order by a field; ascending or descending.
* Limit to a given number of results.
* Offset by a given number.


~~~SECTION:handouts~~~

Notes:

A simple query string might look like

    @@@ Code
    ["and",
      ["=", "type", "User"],
      ["=", "title", "nick"]]'

Documentation at http://docs.puppetlabs.com/puppetdb/latest/api/query/tutorial.html

~~~ENDSECTION~~~


!SLIDE smbullets small

# PuppetDB

Example PuppetDB query:

    @@@ Sh
    $ curl -X GET -H "Accept: application/json" ${CERT_OPTIONS} \
        https://master.puppetlabs.vm:8081/pdb/query/v4/resources \
        --data-urlencode 'query=["and", ["=", "type", "Class"], \
        ["=", "certname", "yourname.localdomain"]]'

A JSON hash is returned:

    @@@ Sh
    [{
      "tags":["class"],
      "file":null,
      "type":"Class",
      "title":"main",
      "line":null,
      "resource":"b33414118184f168ff9476785c716b2242329eeb",
      "environment":"production",
      "certname":"yourname.localdomain",
      "parameters":{"name": "main"},
      "exported":false
    }]


    
~~~SECTION:notes~~~

CERT_OPTIONS could be:

    @@@
    --cacert /etc/puppetlabs/puppet/ssl/certs/ca.pem \
    --cert /etc/puppetlabs/puppet/ssl/certs/yourname.localdomain.pem \
    --key /etc/puppetlabs/puppet/ssl/private_keys/yourname.localdomain.pem
    
~~~ENDSECTION~~~

~~~SECTION:handouts~~~

Notes:

Notice that the Accept header has been updated to save the query as a plain text file:

    @@@ Sh
    $ cat cert_overlord.txt
    ["and",
      ["=", "type", "Class"],
      ["=", "certname", "yourname.localdomain"]]

    $ curl -X GET -H "Accept: application/json" ${CERT_OPTIONS} \
        https://overlord.localdomain:8081/pdb/query/v4/resources \
        --data-urlencode query@cert_overlord.txt

CERT_OPTIONS could be:

    @@@
    --cacert /etc/puppetlabs/puppet/ssl/certs/ca.pem \
    --cert /etc/puppetlabs/puppet/ssl/certs/yourname.localdomain.pem \
    --key /etc/puppetlabs/puppet/ssl/private_keys/yourname.localdomain.pem

~~~ENDSECTION~~~


!SLIDE smbullets
# Lab ~~~SECTION:MAJOR~~~.~~~SECTION:MINOR~~~: List Managed Packages

* Objective:
 * Use curl to list all the packages managed on your node.


!SLIDE supplemental exercises
# Lab ~~~SECTION:MAJOR~~~.~~~SECTION:MINOR~~~: List Managed Packages

## Objective:

****

* Use curl to list all the packages managed on your node.

## Steps:

****

Not required.


!SLIDE supplemental solutions
# Lab ~~~SECTION:MAJOR~~~.~~~SECTION:MINOR~~~: List Managed Packages

****

## Use curl to list all the packages managed on your node.

****

Example file: my_packages.txt

    @@@ Sh
    ["and",
      ["=", "type", "Package"],
      ["=", "certname", "yourname.localdomain"]]

Example file: irssi_package.txt

    @@@ Sh
    ["and",
      ["and",
        ["=", "type", "Package"],
        ["=", "title", "irssi"],
      ["=", "certname", "yourname.localdomain"]]
