rdo-demo-helper
===============

Main scripts:
-------------
prep-base-domain    - Generate a base guest image from a given os type
backup-base-domain  - Backup base guest image for later use
restore-base-domain - Restore base guest image from a backup
rdo-demo-cache      - Install all RDO deps on a base image
rdo-prep-packstack  - Create virtual datacenter
rdo-demo-packstack  - Run Packstack on the virtual datacenter
rdo-demo-save       - Backup virtual datacenter images prior to a demo
rdo-demo-restore    - Restore virtual datacenter images after a demo
rdo-demo            - Run the demo script

Extra scripts:
--------------
os-uninstall        - Remove OpenStack from the current host
gen-image           - Generate a set of images from a base image
gen-answer-file     - Generate a Packstack answer file from template
rdo-reposync        - Cache RDO repos to local host
