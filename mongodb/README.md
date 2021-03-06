MongoDB EC2/Eucalyptus deployment example
=========================================
* Built against Ansible 1.1 
* Assumes RHEL6 or CentOS6 image.  Will add support for other distros in the future.
* Make sure your security group rules allow traffic on port 27017 for MongoDB

This sample playbook demonstrates provisioning of a MongoDB replicaset in Eucalyptus or EC2.  
It uses ephemeral space on instances as the db store.  Use a minimum of two instances, the playbook will scale accordingly.

To execute this playbook, follow these steps:

* Set environmental variables: EC2_URL (if using Eucalyptus or specific endpoint), EC2_ACCESS_KEY (your Euca or AWS access key) and EC2_SECRET_KEY (your Euca or AWS secret key)
* Edit ```mongodb-eucalyptus-ec2.yml``` and alter cloud-specific variables in the first play: ```keypair```, ```instance_type```, ```security_group```, ```image``` and ```count```. Check Ansible's module pages for more ec2-related options.
* Using your private key, run the playbook like so: ```ansible-playbook mongodb-eucalyptus-ec2.yml -v --private-key=/path/to/my/key.pri```
 
Notes on Idempotence
==========================================

This playbook contains three plays, the first is the most significant. It uses the ec2 module to launch an instance and then the add_host module to create a dynamic host group to target this instance for configuration.  If you wish to re-use this playbook without launching new instances, the second and third play should be separated into a new playbook and static hosts targetted (specifically mongos (members of replicaset) and mongopri (first member)).
