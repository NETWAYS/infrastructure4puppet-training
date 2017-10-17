!SLIDE smbullets
# Lab ~~~SECTION:MAJOR~~~.~~~SECTION:MINOR~~~: Deploying Environments

* Objective:
 * Create a git repository on your master and write a hook to deploy environments automaticaly.
* Steps:
 * Create a git repository on your master.
 * Install and configure r10k on the master.
 * Clone, edit and push the repository in a working directory.


!SLIDE supplemental exercises
# Lab ~~~SECTION:MAJOR~~~.~~~SECTION:MINOR~~~: Deploying Environments

## Objective:

****

* Create a git repository on your master and write a hook to deploy environments automaticaly.

## Steps:

****

* Create a git repository on your master.
* Install and configure r10k on the master.
* Clone, edit and push the repository in a working directory.


!SLIDE supplemental solutions
# Lab ~~~SECTION:MAJOR~~~.~~~SECTION:MINOR~~~: Deploying Environments

****

## Create a git repository on your master and write a hook to deploy environments automaticaly.

****

Create a git repository on your master:

    @@@ Sh
    $ yum install -y git
    $ useradd -g puppet -d /var/lib/git -m git
    $ passwd git
    $ su - git
    $ git init —bare puppet.git
    $ cd puppet.git
    $ git symbolic-ref HEAD refs/heads/production

Write a hook to deploy environments via r10k:

    $ vim hooks/post-receive
    #!/bin/bash
    cd /etc/puppetlabs/code/environments || exit 1
    r10k deploy environment -pv

    $ chmod +x hooks/post-receive
    $ exit

Install and configure r10k on the master:

    $ /opt/puppetlabs/puppet/bin/gem install r10k
    $ ln -s /opt/puppetlabs/puppet/bin/r10k /opt/puppetlabs/bin
    $ mkdir /etc/puppetlabs/r10k
    $ vim /etc/puppetlabs/r10k/r10k.yaml
    ---
    :cachedir: /var/cache/r10k
    :sources:
      puppet:
        remote: "/var/lib/git/puppet.git"
        basedir: /etc/puppetlabs/code/environments

    $ install -o git -g puppet -d -m750 /var/cache/r10k
    $ rm -rf /etc/puppetlabs/code/environments/*
    $ chown git:puppet /etc/puppetlabs/code/environments

Create a SSH keypair on the agent machine and autorized the git repository to login:

    @@@ Sh
    $ ssh-keygen -t rsa -b 4096 -N ““
    $ ssh-copy-id git@yourname.localdomain

Clone, edit and push the repository in a working directory on your agent VM:

    @@@ Sh
    $ yum install -y git
    $ git clone git@yourname.localdomain:puppet.git ~/puppetcode
    $ cd ~/puppetcode
    $ git checkout -b production
    $ vim .gitignore
    *.swp
    modules/
    $ git config --global user.name "Your Name"
    $ git config --global user.email your@emailadresse

    $ touch environment.conf
    $ vim Puppetfile
    forge "http://forge.puppetlabs.com"

    mod "puppetlabs/stdlib", "4.5.1"
    mod "puppetlabs/concat"
    mod "puppetlabs/haproxy"

    $ git status
    $ git add environment.conf
    $ git add Puppetfile
    $ git commit -m 'initial commit'
    $ git push origin production

