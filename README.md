ansible influxdb enterprise cluster playbook
=================================

####configuration overview

This is an enterprise installation of influxdb. This playbook is based on the vendor documentation. https://docs.influxdata.com/enterprise/v1.1/production_installation/

####required ansible variables
If these variables are not defined the playbook run will fail:

  * influxdb_ee_license_key - This will set the license key for all enterprise products
  * influxdb_ee_shared_secret - This is required for secure communication between the nodes
  
####adding meta and data nodes must be done manually

    influxd-ctl add-meta meta-01:8091
    influxd-ctl add-data data-01:8088
    influxd-ctl show

####requirements

* [Virtualbox](https://www.virtualbox.org/wiki/Downloads)
* [Vagrant](http://www.vagrantup.com/downloads)
* [Ansible](http://www.ansible.com) via pip install ansible

####running in vagrant

    vagrant up
    bin/provision_development

####destroying

    bin/destroy
