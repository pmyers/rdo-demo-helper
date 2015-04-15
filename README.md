rdo-demo-helper
===============

Some scripts to help with setting up demos of RDO.  Goal is to work with RHEL6,
RHEL7, CentOS6, CentOS7 and Fedora 20+.  All-in-One and multi-node setups work
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

   RHEL 6 qcow2
   https://rhn.redhat.com/rhn/software/channel/downloads/Download.do?cid=16952

   RHEL 7 qcow2
   https://access.redhat.com/downloads/content/69/ver=/rhel---7/7.1/x86_64/product-downloads

   A valid RHN login and RHEL 6 Server subscription is required.

   Place into $VMS_DIR/bak and create a symlinks so that there are links like:
   rhel7-guest.img -> rhel-guest-image-7.0-20140506.1.x86_64.qcow2
   rhel6-guest.img -> rhel-guest-image-6.5-20140523.0.x86_64.qcow2

   NOTE: Using RHEL 6 or 7 implies that you have access to some internal
   repositories at Red Hat.  For non Red Hat users, CentOS 6 or 7 would be
   the way to go, at least until I integrate subscription-manager support in
   the scripts.

3. The script will add entries (via sudo) to /etc/hosts as a convenience

4. git clone https://github.com/pmyers/rdo-demo-helper.git

5. cd rdo-demo-helper

Packstack Instructions:
-----------------------
These instructions are for CentOS 6 but should would similarly for RHEL 6 
and RHEL 7.  Just swap out centos6 below with rhel6 or rhel7.  Note, rhel6
and rhel7 need you to first download the qcow2 guest images.  See instructions
above.

./prep-base-domain centos6

  This creates a zzz-centos6 domain and image, that the other images will be
  based on.

./rdo-demo-cache centos6

  This will populate the base image with all of the packages needed so that
  later runs of the scripts below don't have to access much over the internet

./rdo-prep-packstack centos6 3

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

RHOS Foreman Instructions:
--------------------------
These instructions are for using a RHEL 6.5 VM as the Foreman provisioning
server while the other hosts in the environment are provisioned with RHEL 7.
The RHEL 6.5 qcow2 image is needed for this.  The RHEL 7 machines are
kickstart installed, so you do not need the RHEL 7 qcow2 for these steps.
See the Prerequisites section above for how to put the RHEL 6 qcow2 in the
proper location.

./prep-base-domain rhel6

  This creates a zzz-rhel6 domain and image, that the Foreman node image will
  be based on.

./rhos-prep-foreman rhel6 5

  This creates 1 image backed by the zzz-rhel6 image for installing Foreman on.
  It creates 4 additional VMs with blank disks, that are used for kickstart 
  provisioning RHEL 7.  Note: This also changes the Libvirt network for
  rhel7-mgmt to disable dhcp, so that Foreman can take over this service.

  Five hosts are needed here.  One for Foreman, one for Controller, one for
  Swift, one for Cinder and on for Compute.  You can increase the number
  beyond 5 if you want multiple compute nodes, HA controllers, etc.

./rhos-demo-foreman rhel6 5
 
  This boots the first node in the virtual datacenter, installs Foreman on it
  and runs the foreman installer.  There foreman installer is interactive, so
  some questions will need to be answered.  Details are provided as comments
  in the script.

  After Foreman installs, there are some non-interactive steps run to
  workaround known issues/bugs.  As these bugs are resolved, the hacks will
  be removed.

  Finally, the remaining steps for deploying RHOS on these nodes are manual
  and controlled through the Foreman UI itself.  The comments in the end of
  the script give some details about how to do this.

Notes:
------

* For Packstack, Ceilometer and Sahara are not currently installed due 
  to packaging issues

TODO:
-----

* Try end to end install w/ Fedora 20
* Try devstack install w/ Fedora 20
* Try all-in-one installs with RHEL 6/CentOS 6/Fedora 20
* RHEL 6 images should use subscription-manager with provided login
