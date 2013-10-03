ansible-multi-vms
=================

Support automatic application cluster creation and provision Vagrant and Ansible

First version
=============

* Use ansible check feature to note failure
* Use Vagrant with Digital Ocean
* Use ansible to pass checks
* Create a cluster with a proxy (haproxy), 2 app servers (jetty), a db (mysql) and 1 search engine (elasticsearch)
 
Main commands
=============

* vagrant box add ubuntu-13.04 http://bit.ly/vagrant-lxc-raring64-2013-09-28-
* VAGRANT_DEFAULT_PROVIDER=lxc time vagrant up
* VAGRANT_DEFAULT_PROVIDER=lxc time vagrant reload <vmname>
* VAGRANT_DEFAULT_PROVIDER=lxc time vagrant destroy <vmname>

The time is not mandatory but it gives you an idea of the time take to build the cluster from the ground.
Once downloaded, you may run vagrant reload and note the duration difference.


