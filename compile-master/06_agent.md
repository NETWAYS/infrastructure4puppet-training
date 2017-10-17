!SLIDE smbullets small

# The Agent Service

The puppet service runs on all managed nodes.

* It is responsible for:
 * Sending information about its current state (facts).
 * Requesting configuration state from the Puppet Master.
 * Enforcing a retrieved configuration state (catalog).
* Agent supported platforms include:
 * Linux (RHEL, Debian, and several other distributions)
 * Windows
 * Solaris
 * Mac OS X
 * AIX
 * Network Devices (Arista EOS, Cumulus, ...)


~~~SECTION:handouts~~~

Notes:

The catalog is an object that represents the desired end-state of a node.

Other points to note:

* All communications between the Master and Agent are secured and authenticated via SSL.
* The Agent performs several other ancillary functions:
 * Synchronizing any Puppet extensions from the Master.
 * Retrieving support files as needed from the Master.
 *  Sending a report back to the Master.
 * etc.

~~~ENDSECTION~~~
