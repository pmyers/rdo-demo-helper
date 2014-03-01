rdo-demo-helper
===============

Some scripts to help with setting up demos of RDO

Prerequisites:

0. The user you're running as has sudo enabled and wide open with NOPASSWD

1. Define a VMS_DIR location, i.e.:

   export VMS_DIR=~/vms

   This is should be where you normally put VM images for launching in libvirt
   on the local machine.  Make sure that libvirt is configured to recognize
   this directory as a valid location for storing VM images

2. If using RHEL, download RHEL Guest Image
   
   Place into $VMS_DIR/bak and name like:

   rhel-6-guest.img

   This file in $VMS_DIR/bak can be a symlink.

3. 
