!SLIDE smbullets small

# Tuning ActiveMQ

* ActiveMQ is used as a message broker for orchestration.
* Increase heap only if ActiveMQ service runs out of memory.
* Normally only seen when orchestrating thousands of nodes.
* Default heap size is 512mb.
* ...

System limits for orchestration thousands of nodes:

    @@@ Sh
    $ vim /etc/security/limits.d/30-activemq.conf
    activemq  soft  nproc     8192
    activemq  hard  nproc     8192
    activemq  soft  nofile    16384
    activemq  hard  nofile    16384
