rdo-demo-helper
===============

Some scripts to help with setting up demos of RDO.  Goal is to work with RHEL6,
RHEL7 Beta, CentOS6 and Fedora 20+.  All-in-One and multi-node setups work
using Packstack.  Eventually might try to script Foreman installs too.

Prerequisites:
--------------
0. The user you're running as has sudo enabled and wide open with NOPASSWD

1. Define a VMS_DIR location, i.e.:

   export VMS_DIR=~/vms

   This is should be where you normally put VM images for launching in libvirt
   on the local machine.  Make sure that libvirt is configured to recognize
   this directory as a valid location for storing VM images

2. If using RHEL, download RHEL Guest Image

   https://rhn.redhat.com/rhn/software/channel/downloads/Download.do?cid=16952

   A valid RHN login and RHEL 6 Server subscription is required.

   Place into $VMS_DIR/bak and name like:
   rhel6-guest.img

   This file in $VMS_DIR/bak can be a symlink.

   NOTE: Using rhel6 implies that you have access to some internal
   repositories at Red Hat.  For non Red Hat users, CentOS6 would be the way
   to go, at least until I integrate subscription-manager support in the
   scripts.

3. The script will add entries (via sudo) to /etc/hosts as a convenience

Instructions:
-------------

git clone https://github.com/pmyers/rdo-demo-helper.git

cd rdo-demo-helper

./prep-base-domain centos6

  This creates a zzz-centos6 domain and image, that the other images will be
  based on.

./rdo-demo-cache centos6

  This will populate the base image with all of the packages needed so that
  later runs of the scripts below don't have to access much over the internet

./rdo-demo-prep centos6 3

  This will create 3 virtual machines in a private network and launch them
  in prep for running Packstack or some other installation tool.

./rdo-demo-packstack centos6 3

  This will install packstack and run it on the first node.  The argument
  after 'centos6' indicates you want 3 virtual nodes in the cluster.  Node 1
  is the controller, all 3 will be compute nodes.  Following this, you should
  have a running RDO install

./rdo-demo-save centos6 3

  This can be called after you've installed RDO (via the demo-prep step above)
  and will shut down all of the VMs gracefully and then back them up with a
  .bak extension in the $VMS_DIR location.  Then later you can restore these
  .bak files via:

./rdo-demo-restore centos6 3

./rdo-demo centos6 3

  This will execute an interactive demo that will print out the commands
  being used as well as show lots of CLI output.

./rdo-demo-reset centos6 3

  This just destroys all of the VMs to clean things up

Notes:
------

* Ceilometer and Savanna are not currently installed due to packaging issues
* rdo-deps includes openstack* and so openstack-nova-volume has to be 
  explicitly excluded so that it doesn't get pulled in

TODO:
-----

* DONE: Try end to end install w/ CentOS 6
* Try end to end install w/ Fedora 20
* Try devstack install w/ Fedora 20
* Try all-in-one installs with RHEL 6/CentOS 6/Fedora 20
* Try RHEL 7
* Figure out why Ceilometer doesn't install
* Figure out why Savanna doesn't install
* Experiment with running Foreman instead of Packstack
* RHEL 6 images should use subscription-manager with provided login
