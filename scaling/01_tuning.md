!SLIDE subsection
# ~~~SECTION:MAJOR~~~ Scaling Puppet


!SLIDE smbullets small

# Start With Tuning

The best scaling is scaling you don't need to do.

* Complexity inevitably adds fragility.
* Identify requirements before assuming load balancing is necessary.
* High Availability has a different meaning for configuration management.
 * Agents apply cached catalog if master is unavailable.
 * Restoring master from backup is a short process.
 * New configuration doesn't get propagated for a short period.
* Many parameters for tuning Puppet services.

~~~SECTION:notes~~~

In short: High availability is often requested but rarely needed. Better aim for a fast recovery strategy from master (of masters) loss. The only thing lost while a master of masters is lost is the availability to apply configuration changes.

~~~ENDSECTION~~~
