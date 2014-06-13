rdo-demo-helper
===============

Creating Base Images:
---------------------
prep-base-domain    - Generate a base guest image from a given os type
backup-base-domain  - Backup base guest image for later use
restore-base-domain - Restore base guest image from a backup
rdo-demo-cache      - Install all RDO deps on a base image

Foreman:
--------
rhos-prep-foreman   - Configure network for no dhcp, create Foreman host
                      and additional 'blank' VMs for PXE booting
rhos-demo-foreman   - Install Foreman and kick off an install to provision
                      additional hosts and OpenStack

Packstack:
----------
rdo-prep-packstack  - Create virtual datacenter
rdo-demo-packstack  - Run RDO Packstack on the virtual datacenter
rhos-demo-packstack - Run RHEL OSP Packstack on the virtual datacenter

Saving Environments:
--------------------
rdo-demo-save       - Backup virtual datacenter images prior to a demo
rdo-demo-restore    - Restore virtual datacenter images after a demo

Trying out OpenStack:
---------------------
rdo-demo            - Run the demo script

Extra scripts:
--------------
os-uninstall        - Remove OpenStack from the current host
gen-image           - Generate a set of images from a base image
gen-answer-file     - Generate a Packstack answer file from template
rdo-reposync        - Cache RDO repos to local host
