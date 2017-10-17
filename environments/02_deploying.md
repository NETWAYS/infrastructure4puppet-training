!SLIDE smbullets small

# Puppetfile

Describes the modules to be installed in an environment.

    @@@ Sh
    $ vim /etc/puppetlabs/code/environments/production/Puppetfile
    forge "http://forge.puppetlabs.com"

    mod "puppetlabs/stdlib"
    mod "puppetlabs/ntp", "3.0.0"

    mod "apt",
      :git => "git://github.com/puppetlabs/puppetlabs-apt.git",
      :ref => "testing_branch"

* A Puppetfile lives in the root of a Puppet modulepath.
* Environment will be populated with all the modules listed.
* Eliminates the need to copy external modules into your codebase.
* Best practice is to develop your own modules in separate repositories.

~~~SECTION:handouts~~~

Notes:

Each mod entry in the Puppetfile will provide a module in that environment. These typically come from the Forge or GitHub. Environments can "mix & match" by using the Puppetfile to describe only external modules and continue to maintain a large & monolithic repository containing all internal modules. However, sites who have fully embraced these dynamic ideals will have environments that consist solely of the Puppetfile.

When using a git source, the :ref argument allows you to specify a branch, commit, or tag (anything git can checkout).

Some of the modules this Puppetfile manages include:
* ￼mod "puppetlabs/stdlib"
 * Uses latest razor module from the Forge.
* mod "puppetlabs/ntp", "3.0.0"
 * Uses ntp version 3.0.0 from the Forge.
* The :git argument allow us to fetch module repositories directly from git. If we need a specific commit, release, or tag, we can specify that with the :ref argument.

People often ask why we choose to develop each module in a separate repository. This decouples modules from one another. One module can be developed and tagged with a release completely independently of another. This means that there are fewer chances for conflicting changes, and it helps developers stay in the modular mindset and develop more robust modules that are not tightly coupled to the internals of each other.

~~~ENDSECTION~~~


!SLIDE smbullets small

# Deploying Environmets

Expand a Puppetfile to populate a single environment via r10k:

    @@@ Sh
    $ cd /etc/puppetlabs/code/environments/training
    $ tree
    .
    ├── manifests
    │ └── site.pp
    ├── modules
    └── Puppetfile

    $ r10k puppetfile install
    $ ls modules/
    apt   ntp   stdlib

Typically used to deploy Puppet environments described in a git repository.

* r10k deploy environment -p
* Generates Puppet environments for each branch of a control repository.
* Uses the Puppetfile from each branch to define that environment.


~~~SECTION:handouts~~~

Notes:

The shell transcript shows an empty modules directory that is populated by running r10k's puppetfile install command. It is possible to include modules directly in the control repository in this directory, but this is only recommended for very simple modules. In this case, care must be taken to ensure that the same modules don't exist in the repository and in the Puppetfile.
On new installations, we recommend the use of PE Code Manager, which is the r10k tool integrated into the Puppetserver, with additional features, such as queuing catalog compiles during a codebase deployment to prevent inconsistent catalogs with half-deployed code. The FileSync service will distribute code amongst each master in your infrastructure. This runs on the Puppetserver and exposes a new commit endpoint to trigger deployment.

~~~ENDSECTION~~~
