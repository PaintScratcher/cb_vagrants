#Vagrant files for Couchbase Server VMS

System for quickly and painlessly provisioning Couchbase Server virtual machines across multiple Couchbase versions and OS's.
## Starting a Couchbase cluster

If vagrant and VirtualBox are installed, it is very easy to get started with a 4 node cluster.

See this blog post for more info: http://nitschinger.at/A-Couchbase-Cluster-in-Minutes-with-Vagrant-and-Puppet

Just change into the appropriate directory and call `vagrant up`. Everything else will be done for you, but you need
internet access.

Additionally, you can specify the number of nodes to provision from the command line by using the environment variable VAGRANT_NODES. For example: `VAGRANT_NODES=3 vagrant up` will provision a 3 node cluster. If you do not specify a number a 4 node cluster will be created by default.

**IP Ranges:**

Base range:192.168.xx.10x
* ubuntu10 = 1x
* Ubuntu12 = 2x
* Ubuntu14 = 4x
* Centos5  = 5x
* Centos6  = 6x
* Windows  = 7x


* 1.8.1    = x8
* 2.0.1    = x2
* 2.5.0    = x5
* 2.5.1    = x1
* 3.0.0    = x3

Thus an Ubuntu12 box running 1.8 will have the ip 192.168.28.10x, a Centos5 box running version 2.5.1 will have the ip 192.168.61.10x, simples!

# Building Couchbase

The subdirectory `cbdev_ubuntu_1204` contains a Vagrant configuration for
building Couchbase from source; for Ubuntu 12.04. With this you should be able to build the master branch with the following:

*outside on host*:

    cd cbdev_ubuntu_1204
    vagrant up; vagrant ssh

*inside vagrant VM*:

    mkdir couchbase; cd couchbase
    repo init -u git://github.com/couchbase/manifest -m branch-master.xml
    repo sync
    make
    cd ns_server; ./cluster_run --nodes=1

To build a specific release, change the `branch-master.xml` file to be one of the release files from the [manifests repository][1]. Look for filenames of the form `rel-X.x.x.xml`.
e.g. to build 2.5.1 from source you would change the above `repo init` command to be:

    repo init -u git://github.com/couchbase/manifest -m rel-2.5.1.xml

[1]: https://github.com/couchbase/manifest

See https://github.com/couchbase/tlm/ more information on building Couchbase from source.

## Repo Maintenance
To reduce code duplication and ease maintenance faffery this repo makes heavy use of vagrants ability to join multiple vagrant files together. As such you should only ever have to change the top level Vagrant file and puppet.pp.

### Add a new Couchbase Version:
Clone any of the existing directories and add any required values to the ip_addresses and couchbase_download_links hashes in the top level Vagrant file.

### Add a new OS
Clone any of the existing directories and add an entry in the vagrant_boxes hash in the top level Vagrant file. You may also have to adjust the startup routine in puppet.pp.
