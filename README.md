# Playbook to install Red Hat AMQ

This playbook assumes that you have a fresh install of rhel7 or rhel8

The role packages -> default -> main.yml installs the dependencies required to run Red Hat AMQ. Adjust this to fit your environment. E.g. you may already have a different java installed, you may not need to subscribe the system to get dnf access.

## Instructions
To install the ansible-artemis role

`ansible-galaxy install -r requirements.yml`

 Note this is a fork of the community role in galaxy.
https://galaxy.ansible.com/remyma/ansible-artemis

Credit to the authors, if we improve this at all we will be sure to submit PR

To run the playbook simply replace this with your inventory. Notice the --limit this confines the playbook to the group label_amq_slave this is because the playbook has hosts: all ready for execution in Tower.

`ansible-playbook -i kubevirt.yml  site.yml --limit label_amq_master`

In my environment I am using cnv and I have set metadata labels on the VMI instances in preparation for backup config and clustering.

You will see that site.yml is very succinct simply change the variables in that file to point to your amq binary etc.

All of the detail is in the forked community role
https://github.com/smapjb/ansible-artemis

## Shared Store
Shared store is a role that will configure a gluster shared store for setting up amq as a master slave config

You will need the latest smapjb.ansible-artemis update. To get this you will need to remove the currently downloaded role

`ansible-galaxy remove smapjb.ansible-artemis`

Followed by 

`ansible-galaxy install -r requirements.yml`

The shared store configuration is activated by the existence of groups. Ideally these will be metadata tags or labels on your dynamic inventory.

```
[label_cluster_true]
host1
host2
[label_amq_master]
host1
[label_amq_slave]
host2
```
Replace this role with whatever shared storage tech you want to use