!SLIDE smbullets small

# The Master Service

The puppetserver service runs on the central server.

* It is responsible for:
 * Authenticating agent connections.
 * Signing certificates.
 * Serving a compiled catalog to the agent.
 * Serving files.
 * Processing posted reports.
* Supports several Linux distributions.
* Runs on the JVM for increased performance at scale.
