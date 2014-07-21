#Vagrant files for Couchbase Server VMS

System for quickly and painlessly provisioning Couchbase Server virtual machines across multiple Couchbase versions and OS's.
## Starting a Couchbase cluster

If vagrant and VirtualBox are installed, it is very easy to get started with a 4 node cluster.

See this blog post for more info: http://nitschinger.at/A-Couchbase-Cluster-in-Minutes-with-Vagrant-and-Puppet

Just change into the appropriate directory and call "vagrant up". Everything else will be done for you, but you need
internet access.

## Repo Maintenance
To reduce code duplication and ease maintenance faffery this repo makes heavy use of vagrants ability to join multiple vagrant files together. As such the Vagrantfile located in the bottom directories will draw from both the version file and the top level Vagrantfile. The puppet manifest is taken from the top level puppet.pp.
### Add a new Couchbase Version:
Assuming that the URL structure remains the same, all you have to do is clone any of the current version folders and change the enclosed version file to the correct number

### Add a new OS
Create a new folder in each Couchbase version directory and adjust the enclosed Vagrant file to have the appropriate Vagrant box name and URL.
