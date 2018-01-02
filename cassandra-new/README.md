This is a Readme file which will guide you how to use this role and how it works.

By default this role can be performed in Ubuntu 16.04 for installing latest Cassandra Version 3.9.


playbook.yml - is the sample yaml file which is calling the roles for installing Cassandra and Java for you.

inventery - sample host file where you can mention the IPs of the nodes 
which you want to perform the installation and configurations


defaults - carry variables
templates - carry templates ##for example configuration files, any kind of specif files which you want to put in the destination node
tasks - here we can define the steps to perform in the target machine and you can split it according to the environments
handlers - For handling the state of the application/service


