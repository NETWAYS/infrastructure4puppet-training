!SLIDE smbullets small

# Logfiles

* Puppet agents logs
 * Logged to syslog on Unix variants.
 * Windows Event log.
 * Reports can be viewed in foreman.

* Services in the Puppet Master stack
 * `/var/log/puppetlabs/application/*`
 * example: ￼`/var/log/puppetlabs/puppetdb/*`

* MCollective client logs
 * Depends on the user who runs the client.
 * `~/.mcollective.d/client.log`
