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

* SSL_CERT_FILE=/etc/ssl/certs/ca-certificates.crt DO_CLIENT_ID='<api_client_id>' DO_API_KEY='<api_key>' time vagrant up --provider=digital_ocean <vmname>
* SSL_CERT_FILE=/etc/ssl/certs/ca-certificates.crt DO_CLIENT_ID='<api_client_id>' DO_API_KEY='<api_key>' time vagrant halt -f <vmname>
* SSL_CERT_FILE=/etc/ssl/certs/ca-certificates.crt DO_CLIENT_ID='<api_client_id>' DO_API_KEY='<api_key>' time vagrant destroy <vmname>

The time is not mandatory but it gives you an idea of the time take to build the cluster from the ground.
Once downloaded, you may run vagrant reload and note the duration difference.


