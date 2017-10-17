!SLIDE subsection
# ~~~SECTION:MAJOR~~~ Environment Management


!SLIDE smbullets small

# Environments

Self contained sandboxes

* Allow you to specify multiple self contained zones on a single master.
* Environment naming is arbitrary.
* Default environment is called production.
 * `/etc/puppetlabs/code/environments/production`
 * Created by the puppetserver RPM.
* Often used to track repository branches for development, test and production.


!SLIDE smbullets small

# Directory Environments

    @@@ Sh
    $ tree
    ├── production
    │ ├── environment.conf
    │ ├── manifests
    │ │ └── site.pp
    │ ├── modules
    │ │ └── ...
    ├── feature_brt
    │ ├── manifests
    │ │ └── site.pp
    │ ├── modules
    │ │ └── ...
    └── development
      └── ...

* Environments are created by simply making the directory structure.
* Catalogs compiled for each environment take these settings into account.
* Code in the environment modulepath is used to build the catalog.


~~~SECTION:handouts~~~

Notes:

Older versions of Puppet used classic config file environment definitions. These are no longer supported in current Puppet versions.

    @@@ Sh
    $ vim /etc/puppetlabs/puppet/puppet.conf
    ...
    [production]
      manifest = /etc/puppetlabs/puppet/environments/production/site.pp
      modulepath = /etc/puppetlabs/puppet/environments/production/modules
    [development]
      manifest = /etc/puppetlabs/puppet/environments/development/site.pp
      modulepath = /etc/puppetlabs/puppet/environments/development/modules
    [test]
      manifest = /etc/puppetlabs/puppet/environments/test/site.pp
      modulepath = /etc/puppetlabs/puppet/environments/test/modules

~~~ENDSECTION~~~


!SLIDE smbullets small

# Environment Configuration

Global settings:

* `environment_timeout`
 * How often environment caches expire.
* `basemodulepath`
 * Global modulepath appended to environment.

Per-environment settings:

* `modulepath`
 * Where Puppet searches for modules.
* `manifest`
 * Main manifest or directory.
* `config_version`
 * A script to determine the codebase version.
* `environment_time`
 * Override global cache expiration.


~~~SECTION:handouts~~~

Notes:

Puppet has reasonable defaults, so these settings often need no customization.

For performance, it's suggested to set environments and configure your code deployment process to clear the environment cache as described in https://docs.puppetlabs.com/puppetserver/latest/admin-api/v1/environment-cache.html.

    @@@ Sh
    $ export CACERT=$(puppet master --configprint cacert)
    $ export CERT=$(puppet master --configprint hostcert)
    $ export KEY=$(puppet master --configprint hostprivkey)

Clear all environments:

    @@@ Sh
    $ curl -i --cert ${CERT} --key ${KEY} --cacert $CACERT} -X DELETE \
        https://localhost:8140/puppet-admin-api/v1/environment-cache
    HTTP/1.1 204 No Content

~~~ENDSECTION~~~

!SLIDE smbullets small

# Environment Configuration

Environment location settings:

    @@@ Sh
    $ puppet master --configprint environmentpath
    /etc/puppetlabs/code/environments
    $ puppet master --configprint basemodulepath
    /etc/puppetlabs/code/modules:/opt/puppetlabs/puppet/modules
    $ puppet master --configprint modulepath
    /etc/puppetlabs/code/environments/production/modules: \
    /etc/puppetlabs/code/modules:/opt/puppetlabs/puppet/modules

Example config file environment.conf

    @@@ Sh
    $ vim .../code/environments/production/environment.conf
    # Don't cache the development environment
    environment_timeout = 0

    # Use our custom script to get a
    # git commit for the current state of the code:
    config_version = \
      /usr/local/bin/get_environment_commit.sh $environment


~~~SECTION:handouts~~~

Notes:

The config_version script should use a full path unless it's stored within the environment directory itself. The $environment variable is available for interpolating config_version only.

~~~ENDSECTION~~~

~~~SECTION:notes~~~

Disable the cache on development environments. `puppet agent -t` ignores the cache and so caching will cost resources but you won't get any benefit from it.

~~~ENDSECTION~~~

!SLIDE smbullets small

# Modulepath: How Puppet finds its modules

* A search path for finding modules.
* Default value: `$environment/modules:$basemodulepath`.
* Global modules installed in the `$basemodulepath`.
* Modules in the environment take precedence over global modules.
* Install modules into environments using command line flag.

    <pre>
    $ puppet module install puppetlabs/stdlib --environment development
    Notice: Preparing to install into /.../code/environments/development/modules ...
    Notice: Downloading from https://forgeapi.puppetlabs.com ...
    Notice: Installing -- do not interrupt ... /.../code/environments/development/modules
    └── puppetlabs-stdlib (v4.8.0)
    </pre>

!SLIDE smbullets small

# Custom Plugin Limitations

Puppet extensions in modules:

* Ruby code in modules may leak between environments.
* This is due to the Ruby process model and is indeterminate.
* Only affects Ruby code executed by the master during compilation:
 * If you update a native type and add or remove attributes then you'll get errors about invalid parameters.
 * If you update a native type and the parameter validation changes, then the proper validation may not be used.
 * If you update functions, the wrong version of the function may be executed.
* When developing custom extensions, test on separate standalone masters.


!SLIDE smbullets small

# Agent Specific Environments

* Foreman can be configured to allow the agent to choose an environment.
* Useful for short-lived environment testing.
 * Often the case with feature branch development practices.
* Specify environment on command line:
 * ￼`puppet agent -t --environment env`

Configure permanently in `puppet.conf` on the agent:

    @@@ Sh
    $ vim /etc/puppetlabs/puppet/puppet.conf
    [agent]
      environment = testing_feature


~~~SECTION:handouts~~~

Notes:

Using agent specified environments means that it's possible to assign configuration classes that don't exist in the requested environment. It's up to the user to reconcile that, since the classifier cannot validate class availability.

Foreman Plugin `noenv` in version 0.5 doesn't work with foreman 1.12.

~~~ENDSECTION~~~

~~~SECTION:notes~~~

The precedence of environment settings changed from Puppet 3 to Puppet 4

~~~ENDSECTION~~~
