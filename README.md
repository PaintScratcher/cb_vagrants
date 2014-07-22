#Vagrant files for Couchbase Server VMS

System for quickly and painlessly provisioning Couchbase Server virtual machines across multiple Couchbase versions and OS's.
## Starting a Couchbase cluster

If vagrant and VirtualBox are installed, it is very easy to get started with a 4 node cluster.

See this blog post for more info: http://nitschinger.at/A-Couchbase-Cluster-in-Minutes-with-Vagrant-and-Puppet

Just change into the appropriate directory and call `vagrant up`. Everything else will be done for you, but you need
internet access.

Additionally, you can specify the number of nodes to provision from the command line by using the environment variable n. For example: `n=3 vagrant up` will provision a 3 node cluster. If you do not specify a number a 4 node cluster will be created by default.

## Repo Maintenance
To reduce code duplication and ease maintenance faffery this repo makes heavy use of vagrants ability to join multiple vagrant files together. As such the Vagrantfile located in the bottom directories will draw from both the version file and the top level Vagrantfile. The puppet manifest is taken from the top level puppet.pp.
### Add a new Couchbase Version:
Assuming that the URL structure remains the same, all you have to do is clone any of the current version folders and change the enclosed version file to the correct number. The URL structure defaults to the 2.5.1 style, if you require something different you can specify a seperate URL in the version file.

### Add a new OS
Create a new folder in each Couchbase version directory and adjust the enclosed Vagrant file to have the appropriate Vagrant box name and URL.
